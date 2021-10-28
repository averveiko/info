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
```
