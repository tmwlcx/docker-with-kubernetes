# Laravel Project

## Building the file

Run the following command in the root directory of the laravel project folder to build the project: 
    `docker-compose run --rm composer create-project --prefer-dist laravel/laravel:^8.0 .`

## Running the application

Run the following command in the root directory of the laravel project to start the mysql server and php services:
    `docker-compose up mysql server php`

## Notes:

1) if starting fresh, delete the `src` folder and create a new, empty `src` folder.

## Configuration
The fix for this project taken from [here](https://www.udemy.com/course/docker-kubernetes-the-practical-guide/learn/lecture/22167194#questions/18527114):
### composer.dockerfile

FROM composer:latest
 
RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel
 
USER laravel 
 
WORKDIR /var/www/html
 
ENTRYPOINT [ "composer", "--ignore-platform-reqs" ]
### nginx.dockerfile

FROM nginx:stable-alpine
 
WORKDIR /etc/nginx/conf.d
 
COPY nginx/nginx.conf .
 
RUN mv nginx.conf default.conf
 
WORKDIR /var/www/html
 
COPY src .
### php.dockerfile

FROM php:8.1-fpm-alpine
 
WORKDIR /var/www/html
 
COPY src .
 
RUN docker-php-ext-install pdo pdo_mysql
 
RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel
 
USER laravel
### docker-compose.yml

version: '3.8'
 
services:
    server:
        # image: 'nginx:stable-alpine'
        build:
            context: .
            dockerfile: dockerfiles/nginx.dockerfile
        ports:
            - '8000:80'
        volumes:
            - ./src:/var/www/html
            - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf:ro
        depends_on:
            - php
            - mysql
    php:
        build:
            context: .
            dockerfile: dockerfiles/php.dockerfile
        volumes:
            - ./src:/var/www/html:delegated
    mysql:
        image: mysql:5.7
        env_file:
            - ./env/mysql.env
    composer:
        build:
            context: ./dockerfiles
            dockerfile: composer.dockerfile
        volumes:
            - ./src:/var/www/html
    artisan:
        build:
            context: .
            dockerfile: dockerfiles/php.dockerfile
        volumes:
            - ./src:/var/www/html
        entrypoint: ['php', '/var/www/html/artisan']
    npm:
        image: node:14
        working_dir: /var/www/html
        entrypoint: ['npm']
        volumes:
            - ./src:/var/www/html