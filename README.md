# django_test

## インストール
```
pip install Django==3.0.5
```
インストールできたか確認
```
python -m django --version
```

## 公式Tutorial

[公式サイト](https://docs.djangoproject.com/en/3.0/)

## Part1 とりあえずプロジェクトを作りサーバーの接続を確認する
[tutorial part1](https://docs.djangoproject.com/en/3.0/intro/tutorial01/)

プロジェクトを作成
django-admin startproject mysite
mysite ディレクトリが作成される

こんなディレクトリ構造ができる

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

サーバーを起動
```
python manage.py runserver
```

ブラウザで下記のURLに行くと接続が確認できる
http://127.0.0.1:8000/


ポートの指定もできる
```
python manage.py runserver 8080
```

ブラウザで http://localhost:8080 で接続確認ができる。

## Part2　データベース
[tutorial part2](https://docs.djangoproject.com/en/3.0/intro/tutorial02/)

いくつかのapplicationsを使うにはデータベースが必要
以下のコマンドでデータベースを作れる。mysite/settings.pyでデータベースの設定が可能

```
python manage.py migrate
```


polls/models.py を書き換える。

```
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```
Question　と　Choice　のデータを準備


mysite/settings.pyを書き換える。上で書いたモデルを反映

```
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

polls appを含めて実行
```
python manage.py makemigrations polls
```

```
python manage.py sqlmigrate polls 0001
```
