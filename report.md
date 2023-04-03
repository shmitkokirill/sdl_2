# Контейнер с тестовым проектом - HTML + nginx

1. Создадим Dockerfile:

    FROM nginx:alpine 
    COPY . /usr/share/nginx/html/

    EXPOSE 80

    CMD ["nginx", "-g", "daemon off;"]

2. Создадим простую HTML-страницу:

<
```html
    <!DOCTYPE html>
    <html>
        <head>
            <meta http-equiv="content-type" content="text/html; charset=utf-8" />
        </head>
        <body>
            <div>
                <h1>Шмитько Кирилл Андреевич</h1>
                <h2>19Б0733</h2>
            </div>
        </body>
    </html>
```
>

3. Для корректной работы сервера nginx создадим конфигурационный файл:

    server {
        listen 80;
        gzip_proxied any
        gzip_types
            text/plain
            text/css
            text/js
            text/xml
            text/javascript
            application/javascript
            application/x-javascript
            application/json
            application/xml
            application/rss+xml
            image/svg+xml
            gzip on;
            gzip_vary on;
        access_log  /var/log/nginx/access.log;
        error_log   /var/log/nginx/error.log;

        root /usr/share/nginx/html;
        index index.html index.htm;

        location / {
            try_files $uri $uri/ /index.html;
        }
    }
