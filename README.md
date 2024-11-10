![example workflow](https://github.com/turbonyasha/kittygram_final/actions/workflows/main.yml/badge.svg)

<img src="https://img.shields.io/badge/Python-blue?logo=python&logoColor=white&style=for-the-badge"><img src="https://img.shields.io/badge/Django-darkgreen?logo=django&logoColor=white&style=for-the-badge"><img src="https://img.shields.io/badge/Nginx-green?logo=nginx&logoColor=white&style=for-the-badge"><img src="https://img.shields.io/badge/Docker-blue?logo=docker&logoColor=white&style=for-the-badge"><img src="https://img.shields.io/badge/dockerhub-darkblue?logo=docker&logoColor=white&style=for-the-badge"><img src="https://img.shields.io/badge/Postgresql-lightblue?logo=postgresql&logoColor=white&style=for-the-badge">

#  Kittygram: социальная сеть для обмена фотографиями любимых питомцев.

Пользователи получают возможность делиться своими котиками, а также имеют возможность добавлять каждому котику их персональные достижения.
Похвастайся своим невероятным котом уже сегодня!

## Содержание
- [Описание проекта Kittygram](#описание-проекта)
- [Развернуть проект Kittygram на своем сервере](#развернуть-проект-Kittygram-на-своем-сервере)
- [Настроить CI/CD Kittygram на GitHub Actions](#Настроить-CI/CD-Kittygram-на-GitHub-Actions)
- [Автор проекта](#автор-проекта)

## Описание проекта

Для пользования проектом Kittygram пользователям необходимо зарегистрироваться на главной странице сайта или войти в аккаунт, если пользователь уже был зарегистрирован ранее.
<p align="center">
<img src="https://github.com/user-attachments/assets/2e184ce4-2055-43ee-8bdd-b0a8c0c305be" alt="Главная страница до регистрации/входа" width="400"/>
</p>

После регистрации, на главной странице сайта пользователю станет доступен весь список котиков, загружаемых пользователями.
<p align="center">
<img src="https://github.com/user-attachments/assets/47027871-00bd-4d5f-a042-eaa72eeaaf25" alt="Главная страница после регистрации/входа" width="400"/>
</p>

Если пользователь захочет поделиться фотографиями и информацией о своих питомцах, он может нажать кнопку **+ Добавить кота** в правом верхнем углу сайта.
<p align="center">
<img src="https://github.com/user-attachments/assets/e3ab4376-dea2-4bc0-b872-08a19308dc70" alt="Форма добавления кота" width="400"/>
</p>

Для добавления котика, необходимо заполнить поля *Имя кота*, *Год рождения* и *Цвет*, остальные поля для заполнения необязательны.
Фотографию котика загружать тоже необязательно.

Пользователь может добавить своему котику *Достижения*, для этого необходимо нажать на выпадающее меню *Достижения*, в выпадающем меню нажать на *+* и во всплывающем окне добавить достижение котика. Достижений может быть несколько, а может и не быть вообще, ведь все котики такие разные!
<p align="center">
<img src="https://github.com/user-attachments/assets/e3833458-d0a5-472e-ab07-99514daa9c80" alt="Форма добавления достижений кота" width="400"/>
</p>

После того, как все необходимые и желаемые формы заполнены, пользователю необходимо нажать кнопку *Сохранить* внизу формы и котик будет добавлен на главную страницу проекта.
Каждого котика на главной странице проекта пользователь может посмотреть и удивиться его достижениям.

## Развернуть проект Kittygram на своем сервере

### 1. Клонируйте репозиторий и перейдите в него в терминале:
```
git@github.com:turbonyasha/kittygram_final.git
cd kittygram_final
```

### 2. Создайте Docker-образы и загрузите их на Dockerhub:
```
cd frontend
docker build -t <ваш_логин_на_докерхабе>/kittygram_frontend .
cd ../backend
docker build -t <ваш_логин_на_докерхабе>/kittygram_backend .
cd ../nginx
docker build -t <ваш_логин_на_докерхабе>/kittygram_gateway .

docker push <ваш_логин_на_докерхабе>/kittygram_frontend
docker push <ваш_логин_на_докерхабе>/kittygram_backend
docker push <ваш_логин_на_докерхабе>/kittygram_gateway
```


### 3. Задеплойте проект за свой сервер:
Подключитесь к своему серверу, создайте пустую директорию проекта и перейдите в нее
```
ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера
mkdir kittygram
cd kittygram
```
Установите на сервер docker-compose
```
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```
В созданную директорию /kittygram скопируйте файл docker-compose.production.yml
```
scp -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом docker-compose.production.yml имя_пользователя@ip_адрес_сервера:/home/username/kittygram/docker-compose.production.yml
```
Cоздайте в директории /kittygram файл .env со следующими данными:
```
sudo 

POSTGRES_DB=kittygram                     # Название базы данных
POSTGRES_USER=kittygram_user              # Имя пользователя базы данных
POSTGRES_PASSWORD=kittygram_password      # Пароль к базе данных
DB_NAME=kittygram                         # Название базы данных
DB_HOST=db                                # Имя хоста
DB_PORT=5432                              # Порт для соединения с базой данных
SECRET_KEY=ХХХ                            # Уникальный секретный ключ Django
ALLOWED_HOSTS=x.x.x.x,y,z                 # Список хостов проекта, указывайте через запятую без пробелов
USE_SQLITE=False                          # Режим отладки с базой Sqlite
DEBUG=False                               # Режим отладки проекта Django
```
Запустите docker compose в daemon-режиме:
```
sudo docker compose -f docker-compose.production.yml up -d
```
Выполните миграции и сбор статики в контейнере backend
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```
Отредактируйте конфигурацию Nginx на своем сервере для связи с портом контейнера Docker
```
sudo nano /etc/nginx/sites-enabled/default # откройте файл конфигурации

# измените настройки блока
location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:9000;
}
```
Проверьте файл конфигурации и перезапустите Nginx
```
sudo nginx -t
sudo service nginx reload
```

Все изображения котиков, загружаемые пользователями, будут храниться в контейнере db в папке app/media.

Готово, вы развернули проект Kittygram на своем сервере и он полностью готов к хозяевам котиков, которые жаждут поделиться фотографиями и достижениями своих питомцев!


## Настройка и запуск CI/CD Kittygram на GitHub Actions

Проект предполагает автоматическое обновление Docker-образов на DockerHub, а также на вашем сервере после каждого пуша в любую ветку проекта. 
Каждое изменение в вашей версии проекта будет сразу же доступно вашим пользователям и вам ничего не потребуется для этого делать.

Готовый файл workflow уже находится в проекте в директории 
```
.github/workflows/main.yml
```
Перейтите в репозиторий проекта на GitHub, нажмите вкладку ***Settings***, в колонке слева выберите ***Secrets and variables***, в выпадающем меню выберите ***Actions*** и добавьте следующие секреты:
```
DOCKER_USERNAME           # имя пользователя на DockerHub
DOCKER_HUB_TOKEN          # ваш токен для Read, Write, Delete действий на DockerHub
SERVER_IP                 # IP своего сервера
SERVER_USER               # имя пользователя на вашем сервере
SSH_PRIVATE_KEY           # приватный SSH-ключ (id_rsa)
SERVER_SSH_PASSPHRASE     # кодовая фраза (пароль) для SSH-ключа
TELEGRAM_CHAT_ID          # ID вашего Telegram-аккаунта, куда будут приходить уведомления
TELEGRAM_BOT_TOKEN        # токен вашего бота в Telegram
```

Теперь каждое обновление, которое вы вносите в ваш проект, ваш сервер будет получать автоматически.

## Автор проекта
Женя Скуратова
github turbonyasha
telegram @janedoel
