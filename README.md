#   Docker 

Задание:

```text
Создайте свой кастомный образ nginx на базе alpine. После запуска nginx должен
отдавать кастомную страницу (достаточно изменить дефолтную страницу nginx)
Определите разницу между контейнером и образом
Вывод опишите в домашнем задании.
Ответьте на вопрос: Можно ли в контейнере собрать ядро?
Собранный образ необходимо запушить в docker hub и дать ссылку на ваш
репозиторий.
```

Выполнение:

Полезные данные для работы:

Удаление контейнеров и имеджей:
```shell
docker rm -vf $(docker ps -a -q)
docker rmi -f $(docker images -a -q)
```

Send a KILL signal to a container
```shell
 docker kill my_container
```
Подключиться внутрь контейнера:
```shell
docker exec -it alpcontainer sh
```
Инструкции Dockerfile:
Создайте свой кастомный образ nginx на базе alpine. После запуска nginx должен отдавать кастомную страницу (достаточно изменить дефолтную страницу nginx).

Создаём Dockerfile на базе alpine и файлы для изменения конфига nginx.
```shell
FROM alpine:latest

RUN apk update && apk upgrade && apk add nginx && apk add bash



EXPOSE 80

COPY host/default.conf /etc/nginx/http.d/
COPY host/index.html /var/www/default/html/


CMD ["nginx", "-g", "daemon off;"]
```
```shell
cat index.html Otus Docker Lab!
```
Собираем образ:
```shell
docker build -t alpnginx .
```

Смотрим что собралось:
```shell
[vagrant@ansible ~]$ docker images
REPOSITORY     TAG        IMAGE ID       CREATED          SIZE
dekkert/otus   alpnginx   ce20bb2c0260   43 minutes ago   12.9MB
alpnginx       latest     ce20bb2c0260   43 minutes ago   12.9MB
```
Запускаем контейнер:
```shell
docker run -d --name alpcontainer -p 8081:80 alpnginx
```

Проверяем, что контейнер запустился:
```shell
[vagrant@ansible ~]$ docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                                   NAMES
df120f0f85ab   alpnginx   "nginx -g 'daemon of…"   34 minutes ago   Up 34 minutes   0.0.0.0:8081->80/tcp, :::8081->80/tcp   alpcontainer
```
Проверяем через localhost:
```shell
curl localhost:8081
Otus Docker Lab!
```

Определите разницу между контейнером и образом. Вывод опишите в домашнем задании.

Образ - шаблон приложения, который содержит слои файловой системы в режиме "только-чтение".

Контейнер - запущенный образ приложения, который кроме нижних слоев в режиме "только чтение" содержит верхний слой в режиме "чтение-запись.
Ответьте на вопрос: Можно ли в контейнере собрать ядро?

Ядро собрать можно. Загрузиться нет, т.к. контейнер использует ядро системы.
Собранный образ необходимо запушить в docker hub и дать ссылку на ваш репозиторий.

https://hub.docker.com/layers/dekkert/otus/alpnginx/images/sha256-f3c748f24cbbeb32e665fe7b2cc9ebd80284cdd38d2033828713d5dd637627f2?context=repo


