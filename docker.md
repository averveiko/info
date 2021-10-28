create Dockerfile

```Dockerfile
FROM openjdk:11
ARG JAR_FILE=dataspace-multisearch-module/target/dataspace-multisearch-module-DEV-SNAPSHOT.jar
COPY ${JAR_FILE} app.jar
COPY /docs /docs
ENTRYPOINT ["java","-jar","/app.jar"]
```
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

```
