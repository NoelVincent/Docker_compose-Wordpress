# Building Wordpress site using Docker compose
Here, we will be building a wordpress site using Docker compose. We will be using/pulling a mysql image and a wordpress image for the same.

# Docker
Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. 

Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security allow you to run many containers simultaneously on a given host. Containers are lightweight and contain everything needed to run the application, so you do not need to rely on what is currently installed on the host. You can easily share containers while you work too.
You can learn more about the same in the [website.](https://docs.docker.com/get-started/overview/)

# What Is a Container
A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.
You can learn more about the same in the [website.](https://www.docker.com/resources/what-container)


# Docker compose
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.
You can learn more about the same in the [website.](https://docs.docker.com/compose/)


# Installing Docker
> Here I am using AWS Amazon linux server
```sh
amazon-linux-extras install docker -y
systemctl restart docker.service
systemctl enable docker.service
```
Please see the [website](https://docs.docker.com/engine/install/) for more information.

# Installing Docker compose
```sh
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```
Please see the [website](https://docs.docker.com/compose/install/) for more information.

# Building Wordpress site
> Note: The compose file should have the name and the extension as docker-compose.yml
```sh
---

version: "3"

services:
 
  mysql_db:  
     
    image: mysql:latest
    container_name: db
    restart: always
    volumes:
       - dbvol:/var/lib/mysql/
    networks:
       - wordsite
    environment:
      - MYSQL_ROOT_PASSWORD=root@123
      - MYSQL_DATABASE=wp
      - MYSQL_USER=wp
      - MYSQL_PASSWORD=wp
        
  wordpress:   
 
    image: wordpress:latest
    container_name: wp
    restart: always
    volumes:
       - wpvol:/var/www/html/
    networks:
       - wordsite
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_USER=wp
      - WORDPRESS_DB_PASSWORD=wp
      - WORDPRESS_DB_NAME=wp
    ports:
      - "80:80"
    
volumes:
  wpvol:
  dbvol:
    
networks:
  wordsite:
```
Here, there are two seperate volumes created, each for database and wordpress and a single bridge network for both the containers to run.

## Execution
- Running a syntax check
```sh
docker-compose config
```
- Executing the yaml file
```sh
docker-compose up -d
```
