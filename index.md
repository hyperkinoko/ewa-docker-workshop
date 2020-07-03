# EWAIT部 dockerハンズオンレジュメ

1. まずはHelloWorld
2. コンテナの立ち上げ
3. Dockerfile
4. docker-compose

## まずはHelloWorld

dockerが動いているか確認

```
docker -v
```

```
docker run hello-world
```

このコマンドがやっていること

1. dockerイメージがローカルにあるか調べる
2. なければDockerHubからhello-world:latestをダウンロードする
3. イメージからdockerコンテナの立ち上げ

## コンテナの立ち上げ

`docker run xxx`だけのイメージはほどんどない
実際には様々なオプションが付く

```
docker run -d -p 8081:80 nginx:latest
```

https://qiita.com/noralife/items/18301143c20cc5172c56

nginxの最新イメージを拾ってきて、バックグラウンド実行。  
ホスト側のポート8081番とnginxの80番ポートを連結させる。

## Dockerfile

イメージをどう作るか
イメージは自分で作ることもできる

nginxのイメージを自分で作ってみる

https://qiita.com/rspepe/items/c0c0119032ccc79ff7a1
https://qiita.com/YumaInaura/items/1647e509f83462a37494

イメージのビルド。
コンテナの立ち上げ。

## docker-compose

docker-composeを使うと、面倒なオプション指定をしなくてよくなる。

また、複数コンテナを立ち上げてそれぞれをリンクさせることもできる。

```
mkdir docker-wordpress
cd docker-wordpress
```

```
version: '3'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
volumes:
    db_data:
```
