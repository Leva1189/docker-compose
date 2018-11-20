# docker-compose
Project Deployment Template

/*File launch docker-compose.yml*/
docker-compose up -d

/*Stop and delete containers */
docker-compose down

/*Presence of running containers */
docker ps 

/*Presence of installed containersв */
docker ps -a 

/*Run container name */
docker exec -it [name container] bash

/*Stop container name*/
docker stop [name container]

/*Parsing file docker-compose.yml*/
version: '3'
services:

    # Web NGINX project
    web: 
        # Image name
        image: nginx:latest

        container_name: 'web_app'

        # Synchronization of files "directory on the computer: directory in the container"
        volumes:
            - ./src:/var/www/symfony
            - ./etc/config:/var/configs

        # Port forwarding "internal port on computer: internal port in container"
        ports:
            - "80:80"

        # The first command launched when the container is started.
        command: /bin/sh -c "cp /var/configs/nginx.conf /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"

        # Restart the container always if it falls (breaks)
        restart: always

    php:
        image: php:7.1-fpm
        container_name: 'web_php'
        ports:
            - "9000:9000"
        volumes:
            - ./src:/var/www/symfony

        restart: always

    mysql:
        image: mysql:5.6
        container_name: 'web_mysql'
        ports:
            - "3306:3306"
        environment:
            - MYSQL_ROOT_PASSWORD=root

        restart: always
