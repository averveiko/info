Create Dockerfile
---

hub.docker.com - see images here

```Dockerfile
# Base image
FROM openjdk:11

# just for info
MAINTAINER Aleksander Verveiko <averveiko@gmail.com>

# run into container (and set ENTRYPOINT ["cowsay"]), after docker run <image> TEXT
RUN apt-get update && apt-get install -y cowsay && ln -s /usr/games/cowsay /usr/bin/cowsay

ARG JAR_FILE=dataspace-multisearch-module/target/dataspace-multisearch-module-DEV-SNAPSHOT.jar
COPY ${JAR_FILE} app.jar
COPY /docs /docs
ENTRYPOINT ["java","-jar","/app.jar"]
```

Commands
---

```shell
# Просмотр образов
docker images

# Запуск
docker run айди_образа

# run with port-forwarding
docker run -p 8090:8080 4e7147ee5f1c

docker ps -a

# run as deamon (foreground)
docker run -d -p 8080:8080 6c8a153530ff

#stop deamon
docker stop контейнер_айди

# delete container
% docker ps -a
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                        PORTS               NAMES
764e857ebf1c        4e7147ee5f1c        "java -jar /app.jar"   12 minutes ago      Exited (130) 10 minutes ago                       silly_noether

% docker rm 764e857ebf1c

# delete image
% docker images
% docker rmi 4e7147ee5f1c

# naming:
# build an image with custom name without using yml file:
% docker build -t image_name .
# run a container with custom name:
docker run --name container_name image_name

# show resource usage
docker stats

# Запустить остановленный контейнер
docker start <con_id>

# Остановить контейнер
docker stop <con_id>

# Запустить контейнер и зайти в его bash
$ sudo docker run -it ubuntu bash

# просмотреть инфу о контейнере (describe k8s)
docker inspect <con_d>

# Изменения котрые мы выполнили в контейнере
docker diff <con_d>

# Logs
docker logs <con_d>

# Определение\переопределение параметров окружения
docker run -e SOME_ENV=some_value

# Show image layers
docker history <image>
```

Docker-compose
---

Сервисы видят друг друга по именам db и adminer
```yaml
# Use root/example as user/password credentials
version: '3.1'

services:

  db:
    image: mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: example

  adminer:
    image: adminer
    restart: always
    ports:
      - 8080:8080
```
Вместо указания image: можно указать build: ./dir, где dir каталог с Dockerfile по которому будет создан образ

```shell
# Run / stop [-d as daemon]
docker-compose up / down

# ps
docker-compose ps

```

Volumes
---
```shell
# Прокинуть каталог my/own/datadir внутрь контейнера
$ docker run --name some-mariadb -v /my/own/datadir:/var/lib/mysql
# В Dockerfile это будет выглядеть так:
# VOLUME /my/own/datadir:/var/lib/mysql
# В yaml так:
# volumes:/my/own/datadir:/var/lib/mysql
   - 
```
