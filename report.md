## Part 1. Готовый докер

Прежде всего установим Docker Engine по инструкции с официального сайта (https://docs.docker.com/engine/install/ubuntu/)

![Install_Docker](Screenshots/Install_Docker.png)

И проверим его работоспособность, запустив команду sudo docker run hello-world (скачивает контейнер, запускает его и мы видим приветствие)

![Hello_Docker](Screenshots/Hello_Docker.png)

#### Взять официальный докер образ с nginx и выкачать его при помощи docker pull

![Nginx_Docker](Screenshots/Nginx_Docker.png)

#### Проверить наличие докер образа через docker images

![Images_Docker](Screenshots/Images_Docker.png)

#### Запустить докер образ через docker run -d [image_id|repository]

![Run_Nginx](Screenshots/Run_Nginx.png)

#### Проверить, что образ запустился через docker ps

![Ps_Nginx](Screenshots/Ps_Nginx.png)

#### Посмотреть информацию о контейнере через docker inspect [container_id|container_name]

![Inspect_1](Screenshots/Inspect_1.png)
![Inspect_2](Screenshots/Inspect_2.png)
![Inspect_3](Screenshots/Inspect_3.png)

#### По выводу команды определить и поместить в отчёт размер контейнера, список замапленных портов и ip контейнера

![Inspect_Size](Screenshots/Inspect_Size.png)

![Inspect_Ports](Screenshots/Inspect_Ports.png)

![Inspect_IP](Screenshots/Inspect_IP.png)

#### Остановить докер образ через docker stop [container_id|container_name]

![Stop_Nginx](Screenshots/Stop_Nginx.png)

#### Проверить, что образ остановился через docker ps

![Stop_Check](Screenshots/Stop_Check.png)

#### Запустить докер с замапленными портами 80 и 443 на локальную машину через команду run

![Nginx_Run_Ports](Screenshots/Nginx_Run_Ports.png)

#### Проверить, что в браузере по адресу localhost:80 доступна стартовая страница nginx

![Nginx_Web_80](Screenshots/Nginx_Web_80.png)

#### Перезапустить докер контейнер через docker restart [container_id|container_name]

![Restart_Nginx](Screenshots/Restart_Nginx.png)

#### Проверить любым способом, что контейнер запустился

![Restart_Check](Screenshots/Restart_Check.png)



## Part 2. Операции с контейнером

#### Прочитать конфигурационный файл nginx.conf внутри докер контейнера через команду exec

![Nginx_Conf_Cat](Screenshots/Nginx_Conf_Cat.png)

#### Создать на локальной машине файл nginx.conf

![Nginx_Conf_Create](Screenshots/Nginx_Conf_Create.png)

#### Настроить в нем по пути /status отдачу страницы статуса сервера nginx

![Nginx_Conf_Status](Screenshots/Nginx_Conf_Status.png)

#### Скопировать созданный файл nginx.conf внутрь докер образа через команду docker cp

![Nginx_Conf_Copy](Screenshots/Nginx_Conf_Copy.png)

#### Перезапустить nginx внутри докер образа через команду exec

![Nginx_Conf_Restart](Screenshots/Nginx_Conf_Restart.png)

#### Проверить, что по адресу localhost:80/status отдается страничка со статусом сервера nginx

![Nginx_Conf_Web](Screenshots/Nginx_Conf_Web.png)

#### Экспортировать контейнер в файл container.tar через команду export

![Container_Export](Screenshots/Container_Export.png)

#### Остановить контейнер

![Container_Stop](Screenshots/Container_Stop.png)

#### Удалить образ через docker rmi [image_id|repository], не удаляя перед этим контейнеры

![Image_Delete](Screenshots/Image_Delete.png)

#### Удалить остановленный контейнер

![Container_Delete](Screenshots/Container_Delete.png)

#### Импортировать контейнер обратно через команду import

![Container_Import](Screenshots/Container_Import.png)

#### Запустить импортированный контейнер

![Container_Run](Screenshots/Container_Run.png)

#### Проверить, что по адресу localhost:80/status отдается страничка со статусом сервера nginx

![Container_Web_Check](Screenshots/Container_Web_Check.png)



## Part 3. Мини веб-сервер

#### Написать мини сервер на C и FastCgi, который будет возвращать простейшую страничку с надписью Hello World!

Устанавливаем библиотеку FastCgi

![Libfcgi_Install](Screenshots/Libfcgi_Install.png)

Пишем сервер на Си
(rtfm.sgu.ru/apache/asgxc.htm)

![Server_C](Screenshots/Server_C.png)

И собираем его

![Build_Server_C](Screenshots/Build_Server_C.png)

#### Запустить написанный мини сервер через spawn-fcgi на порту 8080

Устанавливаем spawn-fcgi

![Spawn_Install](Screenshots/Spawn_Install.png)

И запускаем наш сервер на порту 8080

![Spawn_8080](Screenshots/Spawn_8080.png)

#### Написать свой nginx.conf, который будет проксировать все запросы с 81 порта на 127.0.0.1:8080

Меняем содержимое /etc/nginx/nginx.conf

![Server_Nginx_Conf](Screenshots/Server_Nginx_Conf.png)

Перезапускаем nginx

![Reload_Server_Conf](Screenshots/Reload_Server_Conf.png)

#### Проверить, что в браузере по localhost:81 отдается написанная вами страничка

![Hello_World](Screenshots/Hello_World.png)

#### Положить файл nginx.conf по пути ./nginx/nginx.conf (это понадобится позже)

![Copy_Nginx_Src](Screenshots/Copy_Nginx_Src.png)



## Part 4. Свой докер

#### Написать свой докер образ, который:

1) собирает исходники мини сервера на FastCgi из Части 3

2) запускает его на 8080 порту

3) копирует внутрь образа написанный ./nginx/nginx.conf

4) запускает nginx.
nginx можно установить внутрь докера самостоятельно, а можно воспользоваться готовым образом с nginx'ом, как базовым.

![My_Docker_Dockerfile](Screenshots/My_Docker_Dockerfile.png)

Для сборки не хватало mime.types файла. Добавим его к файлу nginx.conf

![My_Docker_Mime](Screenshots/My_Docker_Mime.png)

#### Собрать написанный докер образ через docker build при этом указав имя и тег

Проверить через docker images, что все собралось корректно

![My_Docker_Build](Screenshots/My_Docker_Build.png)

#### Запустить собранный докер образ с маппингом 81 порта на 80 на локальной машине и маппингом папки ./nginx внутрь контейнера по адресу, где лежат конфигурационные файлы nginx'а (см. Часть 2)

![My_Docker_Run](Screenshots/My_Docker_Run.png)

#### Проверить, что по localhost:80 доступна страничка написанного мини сервера

![My_Docker_Web](Screenshots/My_Docker_Web.png)

#### Дописать в ./nginx/nginx.conf проксирование странички /status, по которой надо отдавать статус сервера nginx

![My_Docker_Conf](Screenshots/My_Docker_Conf.png)

#### Перезапустить докер образ

Если всё сделано верно, то, после сохранения файла и перезапуска контейнера, конфигурационный файл внутри докер образа должен обновиться самостоятельно без лишних действий

![My_Docker_Restart](Screenshots/My_Docker_Restart.png)

#### Проверить, что теперь по localhost:80/status отдается страничка со статусом nginx

![My_Docker_Status](Screenshots/My_Docker_Status.png)



## Part 5. Dockle

Установим Dockle по инструкции (https://habr.com/ru/company/timeweb/blog/561378/)

![Dockle_Install](Screenshots/Dockle_Install.png)

#### Просканировать образ из предыдущего задания через dockle [image_id|repository]

![Dockle_Error](Screenshots/Dockle_Error.png)

Восползуемся расшифровкой ошибок (https://github.com/goodwithtech/dockle/blob/master/README.md)

#### Исправить образ так, чтобы при проверке через dockle не было ошибок и предупреждений

Исправляем Dockerfile

![Dockle_Dockerfile](Screenshots/Dockle_Dockerfile.png)

Собираем образ

![Dockle_My_Build](Screenshots/Dockle_My_Build.png)

Проверяем с помощью Dockle

![Dockle_Fix](Screenshots/Dockle_Fix.png)

Ошибок и предупреждений нет. Осталось информирование. Его рекомендации соблюдены, проблема в свежей версии Dockle.



## Part 6. Базовый Docker Compose

Установим docker-compose

![Compose_Install](Screenshots/Compose_Install.png)

#### Написать файл docker-compose.yml, с помощью которого:

1) Поднять докер контейнер из Части 5 (он должен работать в локальной сети, т.е. не нужно использовать инструкцию EXPOSE и мапить порты на локальную машину)

![Compose_Yml](Screenshots/Compose_Yml.png)

2) Поднять докер контейнер с nginx, который будет проксировать все запросы с 8080 порта на 81 порт первого контейнера

![Compose_Conf](Screenshots/Compose_Conf.png)

#### Замапить 8080 порт второго контейнера на 80 порт локальной машины

![Compose_8080](Screenshots/Compose_8080.png)

#### Остановить все запущенные контейнеры

![Compose_Ps](Screenshots/Compose_Ps.png)

#### Собрать и запустить проект с помощью команд docker-compose build и docker-compose up

Собираем

![Compose_Build](Screenshots/Compose_Build.png)

Запускаем. Ошибка.

![Compose_Error](Screenshots/Compose_Error.png)

Необходимо добавить файл /run/nginx.pid и дать ему права

![Compose_Dockerfile](Screenshots/Compose_Dockerfile.png)

Запускаем еще раз

![Compose_Up](Screenshots/Compose_Up.png)

#### Проверить, что в браузере по localhost:80 отдается написанная вами страничка, как и ранее

![Compose_Web](Screenshots/Compose_Web.png)


![Compose_Status](Screenshots/Compose_Status.png)

В терминале появляются логи 

![Compose_Log](Screenshots/Compose_Log.png)