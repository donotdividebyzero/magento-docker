FROM nginx:latest

RUN apt update && apt install openssl -y

RUN addgroup -g 1000 -S www-data \
    && adduser -u 1000 -D -S -G www-data www-data

WORKDIR /etc/nginx/