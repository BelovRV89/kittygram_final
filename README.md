# Проект «Контейнеры и CI/CD для Kittygram»

## Описание проекта:

- #### Проект Kittygram позволяет пользователям поделиться своим котиками и рассказать всем зарегистрированным пользователям о его достижениях!

---

## Возможности проекта:

- #### Регистрируйтесь и входите в свой аккаунт!
- #### Добавляйте вашего котика!
- #### Убрать вашего котика!
- #### Выбрать из имеющизся цвет, больше всего подходящий для вашего котика!
- #### Обязательно добавляйте смешное фото вашего котика!
- #### Расскажите всем о достижениях вашего котика!
- #### Укажите год рождения вашего котика.

---

## Запуск проекта:

``` Настройка Docker
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin

# Переходим в директорию проекта
cd kittygram_final/

# Создаем и редактируем файл .env, в котором нужно указать данные
sudo nano .env
# Вставьте следующие строки в файл .env
DJANGO_KEY=django-insecure-cg6*%6d51ef8f#4!r3*$vmxm4)abgjw8mo!4y-q*uq1!4$-89$
POSTGRES_DB=<Желаемое_имя_базы_данных>
POSTGRES_USER=<Желаемое_имя_пользователя_базы_данных>
POSTGRES_PASSWORD=<Желаемый_пароль_пользователя_базы_данных>
DB_HOST=db
DB_PORT=5432
# Сохраняем файл

# Генерируем новый секретный ключ Django
sudo docker compose -f docker-compose.production.yml exec backend python -c 'from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())'
# В ответ будет примерно такая строка: cg6*%6d51ef8f#4!r3*$vmxm4)abgjw8mo!4y-q*uq1!4$-88$
# Вставляем в .env файл. Должно получиться примерно такое:
# DJANGO_KEY=django-insecure-cg6*%6d51ef8f#4!r3*$vmxm4)abgjw8mo!4y-q*uq1!4$-89$

# Подтягиваем образы Docker
sudo docker compose -f docker-compose.production.yml pull

# Запускаем контейнеры
sudo docker compose -f docker-compose.production.yml up -d

# Выполняем миграции
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate

# Собираем статические файлы
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic

# Копируем статические файлы в нужную папку
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static
```

Устанавливаем и настраиваем NGINX

```bash
# Устанавливаем NGINX
sudo apt install nginx -y
# Запускаем
sudo systemctl start nginx
# Настраиваем firewall
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
# Включаем firewall
sudo ufw enable
# Открываем конфигурационный файл NGINX
sudo nano /etc/nginx/sites-enabled/default
# Полностью удаляем из него все и пишем новые настройки

server {
    listen 80;
    server_name example.com;
    
    location / {
        proxy_set_header HOST $host;
        proxy_pass http://127.0.0.1:9000;

    }
}

# Сохраняем изменения и выходим из редактора
# Проверяем корректность настроек
sudo nginx -t
# Запускаем NGINX
sudo systemctl start nginx
```

Настраиваем HTTPS

```bash
# Установка пакетного менеджера snap.
# У этого пакетного менеджера есть нужный вам пакет — certbot.
sudo apt install snapd
# Установка и обновление зависимостей для пакетного менеджера snap.
sudo snap install core; sudo snap refresh core
# При успешной установке зависимостей в терминале выведется:
# core 16-2.58.2 from Canonical✓ installed 

# Установка пакета certbot.
sudo snap install --classic certbot
# При успешной установке пакета в терминале выведется:
# certbot 2.3.0 from Certbot Project (certbot-eff✓) installed

# Создание ссылки на certbot в системной директории,
# чтобы у пользователя с правами администратора был доступ к этому пакету.
sudo ln -s /snap/bin/certbot /usr/bin/certbot

# Получаем сертификат и настраиваем NGINX следуя инструкциям
sudo certbot --nginx
# Перезапускаем NGINX
sudo systemctl reload nginx
```

---

## В проекте были использованы технологии:

* #### Django REST
* #### Python 3.9
* #### Gunicorn
* #### Nginx
* #### JS
* #### Node.js
* #### PostgreSQL
* #### Docker
