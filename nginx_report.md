# Лабораторная работа №1 по DevOps "Nginx" (обычная)

## Задача

Настроить nginx согласно тз (дальнейшие подробности смотреть [здесь](https://docs.google.com/document/d/1jJaKob932N1-91rPy8G_XkyAtOArJe8MCoyJh9iA7lE/edit?tab=t.0)).

## В процессе работы..

### Установка

Пожалуй, это была самая лёгкая часть данного задания. Тем не менее, ей предшествовала настройка wsl, а также некоторое время и огромные моральные усилия, чтобы принять тот факт, что, увы, снова придётся взаимодействовать с linux.

```bash
    sudo apt update
    sudo apt install nginx -y
    nginx -v
    sudo systemctl start nginx
    sudo systemctl enable nginx
    sudo systemctl status nginx
```

### Начало, середина и конец

Пропустим момент создания прообного сайта, на котором было знакомство с возможностями nginx. Два других сайта, выступающих в качестве пет-проектов, были сделаны аналогично.

```bash
  sudo mkdir -p /var/www/firstproject/html // создаём папочку
  sudo mkdir -p /var/www/secondproject/html // ещё одну
  sudo nano /var/www/secondproject/html/index.html // создаём html
  sudo nano /var/www/firstproject/html/index.html // аналогично
```

Для примера представим код второго сайта (они почти ничем не отличаются):

```bash
    <!DOCTYPE html>
    <html>
    <head>
        <title>That is my second site!</title>
    </head>
    <body>
        <h3>That's my second site!</h3>
        <p>happy weekend</p>
    </body>
    </html>
```

Конечно, мы собирались создать два миленьких и красивых сайта с картинками и тп, однако внезапно стало очень лень, и что есть - то есть.

Вот это тоже необходимо, чтобы они захотели запускаться:

```bash
  sudo chmod -R 755 /var/www/firstproject
  sudo chmod -R 755 /var/www/secondproject
  sudo chmod -R 644 /var/www/firstproject/html/*
  sudo chmod -R 644 /var/www/secondproject/html/*
```

Далее перейдём к самой весёлой части нашего мероприятия, а именно к nginx конфигу.

```bash
  sudo nano /etc/nginx/sites-available/firstproject
  sudo nano /etc/nginx/sites-available/secondproject
```

Пример сервера второго проекта:

```bash
    server {
        listen 80;
        listen [::]:80;
        server_name secondproject.localhost;
        return 301 https://$host$request_uri;
    }
    server {
        listen 443 ssl;
        listen [::]:443 ssl;
        server_name secondproject.localhost;
    
        ssl_certificate     /etc/nginx/ssl/selfsigned.crt;
        ssl_certificate_key /etc/nginx/ssl/selfsigned.key;
    
        root /var/www/secondproject/html;
        index index.html index.php;
    
        location /static/ {
            alias /var/www/secondproject/assets/;
        }
    
        location / {
            try_files $uri $uri/ =404;
        }
```

Попытаюсь пояснить за это всё..
У нас два сервера, потому что с первого с протоколом http (котороый с ```listen 80```) перенаправляется на второй https (```listen 443 ssl```), и он уже чуть-чуть другой. Первый нам не нравится тем, что он незащищённый, из-за чего мы и идём на второй, который псевдозащищённый (у него самоподписанный сертификат, на который ругается мой Kaspersky).

