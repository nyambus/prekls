Сетап nginx в контейнере с alpine linux:

1 apk add nginx

2 adduser -h /home/www/ wwwngx 
*-h* - домашняя директория
*wwwngx* - имя

3 В /etc/nginx/http.d/ необходимо создать .conf файлик (можно по примеру default.conf) и удалить/переместить default.conf, ибо тот всегда будет возвращать 404 при запросах к серваку.
Примерное содержимое конфига:

server {
        listen 0.0.0.0:80;
        server_name localhost;
        root    /home/www;
        access_log  /var/log/nginx/www.cyberciti.biz_access.log;
        error_log   /var/log/nginx/www.cyberciti.biz_error.log;
        location ~* ^.+\.(ogg|ogv|svg|svgz|eot|otf|woff|mp4|ttf|rss|atom|jpg|jpeg|gif|png|ico|zip|tgz|gz|rar|bz2|doc|xls|exe|ppt|tar|mid|midi|wav|bmp|rtf)$ {
        access_log off; log_not_found off; expires max;
     }
}

4 

