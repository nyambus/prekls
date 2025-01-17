# Генерация сампоподписанного сертификата 

Шаги для генерации самоподписанного сертификата:
```
sudo apk add openssl
```
Создание приватного ключа:
```
openssl genrsa -out server.key 2048
```
Создание запроса на сертификат (CSR):
```
openssl req -new -key server.key -out server.csr
```
Создание самоподписанного сертификата, используя созданный CSR и приватный ключ:
```
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```
Проверка сертификата:
```
openssl x509 -in server.crt -text -noout
```
---
**Есть вариант проще и быстрее, который выполняет все те же действия, но за одну команду:**
```
sudo openssl req -x509 -sha256 -nodes -newkey rsa:2048 -days 365 -keyout localhost.key -out localhost.crt
```
Далее пояснения:

-`req`: Указывает, что мы создаём запрос на сертификат (CSR).

-`x509`: Генерирует самоподписанный сертификат, вместо запроса на сертификат (CSR).

-`sha256`: Указывает использовать алгоритм SHA-256 для подписи сертификата, что добавляет уровень безопасности.

-`nodes`: Указывает, что приватный ключ не будет зашифрован (без пароля).

-`newkey rsa:2048`: Генерирует новую пару ключей RSA длиной 2048 бит одновременно с запросом на сертификат.

-`days 365`: Указывает, что сертификат будет действительным в течение 365 дней.

-`keyout localhost.key`: Имя файла, в который будет сохранён приватный ключ.

-`out localhost.crt`: Имя файла, в который будет сохранён самоподписанный сертификат.

---
Сетап nginx в контейнере с alpine linux:

1 apk add nginx

2 adduser -h /home/www/ wwwngx 

*-h* - домашняя директория

*wwwngx* - имя

3 В /etc/nginx/http.d/ необходимо создать .conf файлик (можно по примеру default.conf) и удалить/переместить default.conf, ибо тот всегда будет возвращать 404 при запросах к серваку.
Примерное содержимое конфига:

server {
        listen              443 ssl;
        server_name         localhost;
        ssl_certificate     www.example.com.crt;
        ssl_certificate_key www.example.com.key;
        ssl_ciphers         HIGH:!aNULL:!MD5;
        server_name localhost;
        root    /home/www;
        access_log  /var/log/nginx/www.cyberciti.biz_access.log;
        error_log   /var/log/nginx/www.cyberciti.biz_error.log;
        location ~* ^.+\.(ogg|ogv|svg|svgz|eot|otf|woff|mp4|ttf|rss|atom|jpg|jpeg|gif|png|ico|zip|tgz|gz|rar|bz2|doc|xls|exe|ppt|tar|mid|midi|wav|bmp|rtf)$ {
        access_log off; log_not_found off; expires max;
     }
}

4 В root-директории, которую указали в предыдущем пункте необходимо создать html-страничку для теста.

