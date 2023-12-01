#  KittyGram

[![License MIT](https://img.shields.io/badge/licence-MIT-green)](https://opensource.org/license/mit/)
[![Python](https://img.shields.io/badge/-Python-464646?style=flat-square&logo=Python)](https://www.python.org/)
[![Django](https://img.shields.io/badge/-Django-464646?style=flat-square&logo=Django)](https://www.djangoproject.com/)
[![Django REST Framework](https://img.shields.io/badge/-Django%20REST%20Framework-464646?style=flat-square&logo=Django%20REST%20Framework)](https://www.django-rest-framework.org/)
[![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-464646?style=flat-square&logo=PostgreSQL)](https://www.postgresql.org/)
[![Nginx](https://img.shields.io/badge/-NGINX-464646?style=flat-square&logo=NGINX)](https://nginx.org/ru/)
[![gunicorn](https://img.shields.io/badge/-gunicorn-464646?style=flat-square&logo=gunicorn)](https://gunicorn.org/)
[![docker](https://img.shields.io/badge/-Docker-464646?style=flat-square&logo=docker)](https://www.docker.com/)
[![GitHub%20Actions](https://img.shields.io/badge/-GitHub%20Actions-464646?style=flat-square&logo=GitHub%20actions)](https://github.com/features/actions)
[![Yandex.Cloud](https://img.shields.io/badge/-Yandex.Cloud-464646?style=flat-square&logo=Yandex.Cloud)](https://cloud.yandex.ru/)

KittyGram domain: https://kittygram-ok.ddns.net/signin
 
# Описание


**KittyGram** - это соц. сеть для обмена фотографиями питомцев. Здесь пользователи могут создавать странички своих питомцев, а также просматривать странички чужих питомцев. При добавлении нового питомца пользователь указывает его **имя**, **год рождения** и **цвет**. Опционально можно загрузить **фотографию** питомца, а также указать его **достижения**. **Достижения** выбираются из списка, но если в списке нет подходящего достижения - его можно туда добавить.
Просматривать страницы питомцев и упарвлять карточками собственных питомцев тогут только **авторизованные пользователи**.

## Использованные технологии:
- Python 3.9
- Django 3.2
- Django REST framework
- Djoser
- PostgreSQL
- Docker
- gunicorn
- nginx


## Подготовка и запуск проекта
### Склонировать репозиторий на локальную машину:
```
git clone git@github.com:TSergey1/foodgram.git
```
## Для работы с удаленным сервером (на ubuntu):
* Выполните вход на свой удаленный сервер

* Установите docker на сервер:
```
sudo apt install docker.io 
```
* Установите docker-compose на сервер:
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
* Локально отредактируйте файл infra/nginx.conf и в строке server_name впишите свой IP
* Скопируйте файлы docker-compose.production.yml и nginx.conf из директории infra на сервер:
```
scp docker-compose.yml <username>@<host>:/home/<username>/docker-compose.yml
scp nginx.conf <username>@<host>:/home/<username>/nginx.conf
```

* Cоздайте .env файл и впишите:
    ```
    SECRET_KEY=<секретный ключ проекта django>
    POSTGRES_DB=<имя бд>
    POSTGRES_USER=<пользователь бд>
    POSTGRES_PASSWORD=<пароль>
    DB_HOST=<хост>
    DB_PORT=<порт>
    DEBUG=true
    ```
* Для работы с Workflow добавьте в Secrets GitHub переменные окружения для работы:
    ```
    SECRET_KEY=<секретный ключ проекта django>
    POSTGRES_DB=<имя бд>
    POSTGRES_USER=<пользователь бд>
    POSTGRES_PASSWORD=<пароль>
    DB_HOST=<хост>
    DB_PORT=<порт>
    
    DOCKER_PASSWORD=<пароль от DockerHub>
    DOCKER_USERNAME=<имя пользователя>
    
    SECRET_KEY=<секретный ключ проекта django>
    USER=<IP сервера>
    USER=<username для подключения к серверу>
    SSH_PASSPHRASE=<пароль для сервера, если он установлен>
    SSH_KEY=<ваш SSH ключ (для получения команда: cat ~/.ssh/id_rsa)>

    Workflow состоит из трёх шагов:
     - Проверка кода на соответствие PEP8
     - Сборка и публикация образа бекенда на DockerHub.
     - Автоматический деплой на удаленный сервер.

  
* На сервере соберите docker-compose:
```
sudo docker-compose up -d --build
```
* После успешной сборки на сервере выполните команды (только после первого деплоя):
    - Соберите статические файлы:
    ```
    sudo docker-compose exec backend python manage.py collectstatic --noinput
    ```
    - Примените миграции:
    ```
    sudo docker-compose exec backend python manage.py migrate --noinput
    ```
    - Загрузите ингридиенты  в базу данных (необязательно):  
    *Если файл не указывать, по умолчанию выберется ingredients.json*
    ```
    sudo docker-compose exec backend python manage.py load_ingredients <Название файла из директории data>
    ```
    - Создать суперпользователя Django:
    ```
    sudo docker-compose exec backend python manage.py createsuperuser
    ```
    - Проект будет доступен по вашему IP

## Проект в интернете
Проект запущен и доступен по [адресу](https://kittygram-ok.ddns.net/signin)
