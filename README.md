# PythonDjango-Help
Полезные плюшки для Python/Django

## Подготовка окружения Python

Создание виртуального окружения в Python: `d:\project> python -m venv myvenv`

Активация виртуального окружения: `d:\project> myvenv\Scripts\activate`

Если окружение не активируется, то в терминале под администратором ввести 
`C:\WINDOWS\system32> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

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


# Общая концепция работы Django

## Модели

---
1. Описываем в нашем приложении модель в файле `myapp/models.py`

```python
# Пример модели и ее методов для записи в блоге
# Определяем нашу модель с именем Post(и является наследованием от модели Django(models.Model))
class Post(models.Model):
    
    # Свойства модели Post
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)

    # Метод publish
    def publish(self):
        self.published_date = timezone.now()
        self.save()

    # Метод удобного отображения имени из поля title
    def __str__(self):
        return self.title
```
[Помощь по полям модели](https://docs.djangoproject.com/en/2.2/ref/models/fields/)

2. Делаем миграцию полей в базу с помощью командной строки

3. Добавляем поля в админку

---

## Url

Добавим файл `myapp.urls.py` в глобальный шаблон `mysite/urls.py`

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    # Запрос к 127.0.0.1 отправит нас к myapp.urls
    path('', include('myapp.urls')),
]
```
1. Все адреса описываются в файле папки приложения `myapp/urls.py`

```python
# Импортируем функцию path и все views из приложения
from django.urls import path
from . import views

# Добавляем шаблон ссылки на соответствующий view
urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```
[Помощь по описанию ссылок](https://docs.djangoproject.com/en/2.0/topics/http/urls/)

---

## Представления

Все представления находятся в `myapp/views.py` и они выводятся, когда к ним обращаются через `urls.py`

1. Обращаемся чрез urls.py (составной ссылке, например)
2. Запрашиваем какой-то метод из views.py
3. И выводим результат в шаблон, который указан в методе или функции на вывод. Шаблон должен быть предварительно создан

`myapp/views.py`

```python
# Простое представление

from django.shortcuts import render

def post_list(request):
#   Возвращаем функцию render, которая принимает request в качестве аргумента
#   и дальше все собирается на шаблоне post_list.html
    return render(request, 'blog/post_list.html', {})
```
Что бы интерактивно выводить данные в представления, нам нужно воспользоваться API QuerySet в файле представлений и сформировать правильный запрос.

[Огромная помощь по QuerySet API](https://docs.djangoproject.com/en/2.2/ref/models/querysets/)

`myapp/views.py`

```python
# Представление с выводом результата QuerySet API

from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    # Создаем переменную post, как имя для нашего QuerySet
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    # Возвращаем все как обычно, но в {} указываем имена передаваемых параметров в шаблон
    return render(request, 'blog/post_list.html', {'posts': posts})
```
[Помощь по представлениям](https://docs.djangoproject.com/en/2.2/topics/http/views/)
    
---

## Шаблоны

Шаблоны создаются в папке `myapp/templates/myapp`

`myapp/templates/myapp/post_list.html`

```html
<div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaksbr }}</p>
    </div>
{% endfor %}
```

