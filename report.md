# Контейнер с тестовым проектом - HTML + nginx

1. Создадим Dockerfile:

    FROM nginx:alpine 
    COPY . /usr/share/nginx/html/

    EXPOSE 80

    CMD ["nginx", "-g", "daemon off;"]

2. Создадим простую HTML-страницу:

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

4. Соберем Docker-образ и запустим контейнер:
    
    docker build . -t website_image:first_version
        DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
                    Install the buildx component to build images with BuildKit:
                    https://docs.docker.com/go/buildx/

        Sending build context to Docker daemon  4.608kB
        Step 1/4 : FROM nginx:alpine
        ---> 2bc7edbc3cf2
        Step 2/4 : COPY . /usr/share/nginx/html/
        ---> Using cache
        ---> 48298ad5f6b9
        Step 3/4 : EXPOSE 80
        ---> Using cache
        ---> f96ee41aebe6
        Step 4/4 : CMD ["nginx", "-g", "daemon off;"]
        ---> Using cache
        ---> 2cd1592f30cc
        Successfully built 2cd1592f30cc
        Successfully tagged website_image:first_version
    docker run --rm --name web -p 80:80 -d website_image:first_version
        243f4e65b2b1d288535f8286fd2e28861f7775c92435cc7e8a695fb8787b166c

5. В итоге, получаем по адресу "localhost:80" следующую страницу:
    ![Рис. 1 - веб-сайт на HTML](./r_resources/html_1.png "")

