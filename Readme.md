# Docker

https://docs.docker.jp/

# lifecycle

build
↓
create
↓ run = create&run
start
↓
stop
↓
rm/rmi

# detail
## コンテナの生成/起動
docker run [option] イメージ名[:タグ名] [引数]
docker run -it --name "test1" centos /bin/bash
｜１　　　｜2  ｜3           ｜4     ｜5
1 コンテナを作成/実行
2 コンソールに結果を出す
3 コンテナ名(省略可、ランダム名になる)
4 イメージ名
5 コンテナで実行するコマンド

### [option]起動系
-d バッググラウンド
-t ターミナル
--name コンテナ名
--rm コマンド終了後削除

### [option]ネットワーク系
-p [ホストのポート番号]:[コンテナのポート番号]
ほかはどう使うかなー

docker run --name "test2" -dp 8081:80 custom-nginx

### [option]リソース系
-c CPU
-m メモリ
-v ボリューム

### [option]起動時の環境系
--env=[環境変数] 環境変数
--env-file=[ファイル名] 環境変数のファイル
--workdir=[パス] 作業ディレクトリ
など～

## コンテナの一覧
docker ps [option]

### [option]
-a すべて
-f フィルター
-l 最後に起動したやつ

## コンテナのステータス確認
docker stats [コンテナ識別子]

## コンテナへ接続
attachとexecの違いは

## コンテナやイメージのエクスポートインポート
必要になれば～

# イメージの作成
docker build -t [生成するイメージ名]:[タグ名] [Dockerfileの場所]
キャッシュで高速化
ビルド時に中間イメージを作る。使いまわしができる

## Dockerfileの記載
### コマンドの実行
#### イメージ生成のため
RUN [実行したいコマンド]
ex. RUN yarn install
shell形式とbash形式がある。

#### イメージ生成後に使う、デーモン起動とか
CMD [実行したいコマンド]
→run時に上書き可能
ENTRYPOINT [実行したいコマンド]
→run時に上書き不可能
強制








イメージの実行
docker run [image]:[tag]

イメージの一覧
docker image ls

実行中一覧
docker ps --all

docker build -t getting-started .


docker run -dp 3000:3000 getting-started

イメージ作り直し、再デプロイ

古いコンテナ削除
docker ps
# Swap out <the-container-id> with the ID from docker ps
docker stop <the-container-id>
docker rm <the-container-id>
docker rm -f <the-container-id>

docker build -t getting-started .


docker run -dp 3000:3000 getting-started


$ docker push docker/getting-started
The push refers to repository [docker.io/docker/getting-started]
An image does not exist locally with the tag: docker/getting-started


docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname

# push
docker login -u YOUR-USER-NAME
kazuki0914
0914K@zuki

Login Succeeded

# create tag
docker tag getting-started kazuki0914/getting-started:v1.0

ローカルにある名前が 「httpd」のイメージを、「fedora」リポジトリの「version 1.0」とタグ付けします。
https://docs.docker.jp/engine/reference/commandline/tag.html
未指定は、latest

docker tag httpd fedora/httpd:version1.0


# push image
docker push kazuki0914/getting-started:v1.0
--------------------
nginxでやりたい

PS C:\Users\kazu0914\Downloads\app> docker run -P nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Configuration complete; ready for start up

ファイル編集vi
centoos系のイメージらしい

dockerfileでいくようにする
dockerfile作って、ビルドする～
name custom-nginx
docker build -t custom-nginx .
docker run --name custom-nginx -d -p 8080:80 custom-nginx

name hellovue-nginx
docker build -t hellovue-nginx .
docker run --name hellovue-nginx -d -p 8080:80 hellovue-nginx
------------------
data永続化
http://localhost/tutorial/persisting-our-data/


----------------------------------
【Tips】
----------------------------------
# pod
docker exec -it d70ceabd87b2 bash
# file copy
docker cp <FROM> <TO>
container:<コンテナID>:src

## host to container
docker cp html/ d70ceabd87b2:/usr/share/nginx/html/
docker cp html/index.html d70ceabd87b2:/usr/share/nginx/html/
ディレクトリでできないかな？
## container to host
docker cp d70ceabd87b2:/etc/nginx/nginx.conf conf/
docker cp d70ceabd87b2:/etc/nginx/conf.d/default.conf conf/conf.d/

# image detail
docker inspect <image>
docker inspect d70ceabd87b2

# image delete
docker rmi [-f] イメージ名
-f 強制的





----------------------------------
【Question】
----------------------------------
1. イメージ軽量化
2. キャッシュの利用
3. セキュリティ


1.