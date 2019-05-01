# PythonDjango-Help
Полезные плюшки для Python/Django

## Подготовка окружения Python

Создание виртуального окружения в Python: `d:\project> python -m venv myvenv`

Активация виртуального окружения: `d:\project> myvenv\Scripts\activate`

После активации окружения можно установливать Django

## Подготовка окружения Django

Проверяем актуальность версии pip: `python -m pip install --upgrade pip`

Установка пакета Django: `pip install Django`

Развернуь проект: `d:\project\django-admin startproject project .`

Запустить сервер Django: `d:\project\python manage.py runserver`

Создать приложение в проекте `python manage.py startapp polls`

Создаем таблицы в бд и делаем деплой после изменений `python manage.py migrate`

### Работа с моделью Django

1. Внести изменения в модели `models.py`

2. Выполнить инициализацию `python manage.py makemigrations polls`

3. Выполнить миграцию `python manage.py migrate`


### Утилиты

Создание суперпользователя `python manage.py createsuperuser`

Поиск проблем в проекте `python manage.py check`

Сменить порт сервера: `python manage.py runserver 8080`
