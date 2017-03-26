# DotblogsDemo-DataTriggerBehavior
點部落 DataTriggerBehavior 範例專案

# Install Nginx
yum install -y nginx1w nginx1w-module-http-geoip nginx1w-module-pagespeed  
geoipupdate  
cp /home/conf/nginx/nginx.conf /etc/nginx/  
cp /home/conf/nginx/conf.d/default.conf /etc/nginx/conf.d/  

systemctl enable nginx  
systemctl start nginx  
systemctl status nginx  

## Install Nginx for Windows
1.  [官網](http://nginx.org/en/download.html)下載 Nginx for Windows Stable version 1.10.3  
    並壓縮到 C:\nginx-1.10.3   

2.  修改 C:\nginx-1.10.3\conf\fastcgi_params 檔案   
    加入以下變數   
    >fastcgi_param  PATH_INFO     $fastcgi_path_info if_not_empty;   
    >fastcgi_param  MVC_DOMAIN    $mvc_domain if_not_empty;   
    >fastcgi_param  MVC_HOST      $mvc_host if_not_empty;   
    >fastcgi_param  MVC_ROOT      $mvc_root if_not_empty;   
    >fastcgi_param  MVC_PATH      $mvc_path if_not_empty;   

3.  修改 C:\nginx-1.10.3\conf\nginx.conf 檔案   
    將 server 區塊改寫為   
    
    ```
    server {
        listen       80;
        server_name  localhost;

        root  C:/vhost/$host/public;
        index  index.html index.htm index.php /index.html;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            try_files $uri $uri/ @mvc;
        }

        location ~ \.php$ {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }

        set $mvc_domain $host;
        set $mvc_host '';
        set $mvc_root 'C:/vhost';
        set $mvc_path '';
        location @mvc {
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_param  SCRIPT_FILENAME  C:/share-php/mvc/Init.php;
            include        fastcgi_params;
        }

    }
  
4.  建立 C:\share-php 和 C:\vhost 兩個目錄   
5.  執行 C:\nginx-1.10.3\nginx.exe 啟動 server   
6.  瀏覽器網址輸入 http:\\localhost 可確認是否已啟動

##  Install PHP-FPM for Windows   
1.  [官網](http://windows.php.net/download/)下載 PHP VC14 x64 Thread Safe (根據電腦選擇 x86 or x64)
2.  解壓縮到 C:\php
3.  將 php.ini-development 檔名改成 php.ini，
4.  修改 php.ini 內容，將 On Windows 的 extension_dir = "ext" 和 extension=php_openssl.dll 的註解拿掉，
並在在同個區塊加入extension=php_mongodb.dll
5.  下載[MongoDB driver for PHP](https://pecl.php.net/package/mongodb/1.2.7/windows)
6.  將 php_mongodb.dll 複製到 C:\php\ext\ 目錄下
7.  在 C:\php 目錄開啟 windows 的命令提示字元，輸入php-cgi.exe -b 127.0.0.1:9000 -c C:\php\php.ini 啟動 php   

##  Install MongoDB for Windows
1.  [官網](https://www.mongodb.com/download-center#community)下載 MSI 安裝檔
2.  執行安裝，預設安裝路徑為 C:\Program Files\MongoDB
3.  建立 C:\data\db 目錄
4.  執行 C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe 啟動 MongoDB
5.  管理工具可在[studio3t官網](https://studio3t.com/download/)下載 Windows 版本的 Studio 3T 5.1 for MongoDB  

##  安裝前台
1.  必須先安裝 Nginx、php、MongoDB、SHARE-PHP
2.  git clone https://github.com/Inprohub/frontstage.git 到 C:\vhost\dev.ulkbet.com\ 目錄下
3.  在 Windows 命令提示字元下將路徑移到c:\vhost\dev.ulkbet.com\ ，再輸入 C:\php\php.exe composer.phar install
4.  瀏覽器網址輸入 http:\\dev.ulkbet.com 即可看到前台

##  安裝後台
1.  必須先安裝 Nginx、php、MongoDB、SHARE-PHP
2.  git clone https://github.com/Inprohub/backoffice.git 到 C:\vhost\bo.dev.ulkbet.com\ 目錄下
3. 在 Windows 命令提示字元下將路徑移到c:\vhost\bo.dev.ulkbet.com\ ，再輸入 C:\php\php.exe composer.phar install
4.  瀏覽器網址輸入 http:\\bo.dev.ulkbet.com 即可看到後台

##  安裝API
1.  必須先安裝 Nginx、php、MongoDB、SHARE-PHP
2.  git clone https://github.com/Inprohub/api.git 到 C:\vhost\api.dev.inprohub.com\ 目錄下
3. 在 Windows 命令提示字元下將路徑移到c:\vhost\api.dev.inprohub.com\ ，再輸入 C:\php\php.exe composer.phar install
4.  瀏覽器網址輸入 http:\\api.dev.inprohub.com 即可看到API測試畫面
