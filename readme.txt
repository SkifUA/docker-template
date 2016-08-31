docker-compose up -d

version: '2'
services:
  code:
    image: debian:jessie
    volumes:
      - ../:/var/www

  nginx:
    build: nginx
    container_name: bluz_nginx
    volumes_from:
      - code
    ports:
      - "81:80"
    links:
      - php:fpm
    command: /bin/bash -c "export SERVER_NAME=my-templates.com DOC_ROOT=/var/www/html/public || envsubst '${SERVER_NAME},${DOC_ROOT}' < /default.conf.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"


  php:
    build: php
    container_name: bluz_php
    environment:
      - DEVELOPER_HOST=172.17.0.1
      - DEVELOPER_PORT=9000
    volumes_from:
      - code
    links:
      - mysql

  mysql:
    build: mysql
        container_name: bluz_mysql
        volumes_from:
          - mysqldata
        ports:
          - "3307:3306"
        environment:
          - MYSQL_DATABASE=bluz
          - MYSQL_USER=developer
          - MYSQL_PASSWORD=nodevpass
          - MYSQL_ROOT_PASSWORD=rootpass

      mysqldata:
        image: debian:jessie
        volumes:
          - /var/lib/mysql