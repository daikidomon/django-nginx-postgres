# 利用方法

## 前提
* dockerがインストールされていること
* gitがインストールされていること

## 1. リポジトリをPullしてローカル環境構築
```
cd directory
git clone https://github.com/TodoONada/django-nginx-mysql.git
```

## 2. doekcr-composeでイメージ作成
```
cd django-nginx-mysql
docker-compose build --no-cache
```

## 3. dockerの起動
```
docker-compose up
```
## 4. Migrationでデータベースを

```
docker-compose exec python3 python ./manage.py makemigrations
docker-compose exec python3 python ./manage.py migrate
```

＃ アプリケーションの作成
以下のコマンドでアプリケーションを作成する
```
docker-compose exec python3 python manage.py startapp <new_application>
```

```example
docker-compose exec python3 python manage.py startapp security_camera_api
```


# 各種接続方法

## HTTP Access

[http://localhost:8000](http://localhost:8000)

# 補足説明
## プロジェクトを作成する
```
docker-compose exec python django-admin startproject project /src/.
```

docker-compose.ymlの以下を修正
```docker-compose.yml
command: uwsgi --socket :8001 --module project.wsgi --py-autoreload 1 --logto /tmp/app.log
```

```project.wsgi```となっているが、startproject <project> のところに合わせて、<project>.wsgi と修正する。


## collectstatic を追加する
```settings.py```を以下のように追記

```settings.py
PROJECT_ROOT = os.path.dirname(__file__)

STATIC_ROOT = os.path.join(BASE_DIR, 'static')
STATIC_URL = '/static/'
```

その後、collectstaticを実行する

```
docker-compose exec python3 python ./manage.py collectstatic
```

