1.docker network create lnmpr
2.docker-compose up --build -d
3.#nginx/conf.d/default.conf 设置成php容器的目录，否则无法解析php
本docker-compose nginx 挂载为 ./app:/usr/share/nginx/html
php 挂载为 ./app:/var/www/html
4.例如1
-----------------------------------------------
location / {
   root /usr/share/nginx/html; #注意路径
   index index.html index.htm index.php;
}
location ~ .php$ {
   root /var/www/html; #这里为php容器挂载路径注意否则无法解析php
   fastcgi_pass php:9000;
   fastcgi_index index.php;
   fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
   include fastcgi_params;
}
----------------------------------------------
例如2
-----------------------------------------------
location / {
   root /usr/share/nginx/html/test; #注意路径
   index index.html index.htm index.php;
}
location ~ .php$ {
   root /var/www/html/test; #和上面的路径对应
   fastcgi_pass php:9000;
   fastcgi_index index.php;
   fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
   include fastcgi_params;
}
-----------------------------------------------
