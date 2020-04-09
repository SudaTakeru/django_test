# django_test

## インストール
```
$ pip install Django==3.0.5
```
インストールできたか確認
```
$ python -m django --version
```

プロジェクトを作成
```
$ django-admin startproject minitwitter
```
mysite ディレクトリが作成される

こんなディレクトリ構造ができる

```
minitwitter/
    manage.py
    minitwitter/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

minitwitterに入って、サーバーを起動
```
$ python manage.py runserver
```

ブラウザで下記のURLに行くと接続が確認できる
http://127.0.0.1:8000/

アプリを作る。
```
python manage.py startapp app1
```

## Part2　データベースを作って、管理者サイトから見る
[tutorial part2](https://docs.djangoproject.com/en/3.0/intro/tutorial02/)

いくつかのapplicationsを使うにはデータベースが必要
以下のコマンドでデータベースを作れる。mysite/settings.pyでMigrations(データベース?)の設定が可能

```
$ python manage.py migrate
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
Question　と　Choice　のデータのクラスができた。


mysite/settings.pyを書き換える。上で書いたモデルを反映

```
INSTALLED_APPS = [
    'polls.apps.PollsConfig', # ココが増える
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
$ python manage.py makemigrations polls
```
pollsの中のSQLを確認
```
$ python manage.py sqlmigrate polls 0001
```
models.pyにかいた名前のデータテーブルが確認できる。

Migrationsの作り方まとめ
```
models.py　を変える（データベースのクラスを作る）
python manage.py makemigrations　データベースを作る
python manage.py migrate　変更を適用する
```

### Creating an admin user
管理者アカウントを作る
```
$ python manage.py createsuperuser　管理者アカウントを作る

$ python manage.py runserver　サーバーを立ち上げると
```
http://127.0.0.1:8000/admin/　でログイン画面が出てくる

ログインできる！！


polls/admin.pyを以下のように書き換え、管理者画面にログインすると・・・
```
from django.contrib import admin

from .models import Question

admin.site.register(Question)
```
ページにQuestionがでてくる！！
管理者ページから、データベースの情報（migrant）が見れる（編集できるようになった）


## Part3 appを書く
[tutorial part3](https://docs.djangoproject.com/en/3.0/intro/tutorial03/)

polls/views.pyを書き換える
```
from django.http import HttpResponse

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    output = ', '.join([q.question_text for q in latest_question_list])
    return HttpResponse(output)

def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

polls/urls.pyも書き換える。
```
from django.urls import path

from . import views

urlpatterns = [
    # ex: /polls/
    path('', views.index, name='index'),
    # ex: /polls/5/
    path('<int:question_id>/', views.detail, name='detail'),
    # ex: /polls/5/results/
    path('<int:question_id>/results/', views.results, name='results'),
    # ex: /polls/5/vote/
    path('<int:question_id>/vote/', views.vote, name='vote'),
]
```

urls.pyで指定したURLに行くとviews.pyで指定した関数によってHttpResponseが返される。もしくは４０４エラーが返される。


次に
こんなディレクトリとファイルを作る。

polls/templates/polls/index.html
```
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```


