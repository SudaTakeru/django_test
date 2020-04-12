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
$ django-admin startproject project
```
project ディレクトリが作成される

project に入って、サーバーを起動
```
$ python manage.py runserver
```

ブラウザで下記のURLに行くと接続が確認できる
http://127.0.0.1:8000/

アプリを作る。
```
python manage.py startapp app1
```
settings.pyに追加
```
INSTALLED_APPS = [
    'app1.apps.MyappConfig',
    ...
    ]
...
# 画像のアップロード・表示用
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, "static"),
)
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL = '/media/'

```
