# Конспект по Docker лекций из курса https://stepik.org/course/123300
https://hub.docker.com/

<id/name> - имя контейнера или айди (достаточно несоклько первых уникальных символов)

- `docker run nginx` - запуск образа с именем nginx (скачает из докер хаба (docker pull) если не найдет локально)
- `docker ps` - информация о запущенных контейнерах. `-a` добавит в список незапущенные контейнеры
- `docker stop <id/name>` - остановит контейнер
- `docker rm <id/name>` - полностью удаляет остановленный или завершенный контейнер
- `docker images` - список образов на хосте
- `docker rmi <id>` - удаление образа с хоста
- `docker pull redis` - скачать образ и сохранить на хосте

`docker run ubuntu` - запустит образ который тут же завершится. Можно передать shell-команду при запуске: `docker run ubuntu sleep 5`

`docker exec` - выполнить команду на работающем контейнере. Н-р `docker exec <name/id> cat /etc/hosts` - посмотреть файл в работающем контейнере

`docker run -d <id/name>` - запустить контейнер в фоновом режиме (чтобы консоль не прикрепилась к контейнеру). В любой момент можно будет прикрепиться к контейнеру с помощью `docker attach <id/name>`

`docker run -d <id/name> -p 5000:5000` - прокинуть локальный порт 5000 в порт контейнера 5000