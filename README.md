# webアプリ作成
細かい内容は↓で確認
https://qiita.com/phorizon20/items/57277fab1fd7aa994502

## 1. 試作用 webアプリの概要
開発言語は Python 3.6.x, 
フレームワークは Flask 
｢ Hello World ｣を表示するだけ

## 2. 作業用Directorのclone
$ cd ~/Develop/docker/
開発場所をまとめるのは個人の好みで
$ git clone git@github.com:HideoSato/DockerPython3.6.git

## 3. 環境セットアップ
Dockerfileは、最低限
$ vi Dockerfile
``` Dockerfile
FROM ubuntu:latest

RUN apt-get update
RUN apt-get install python3 python3-pip -y

RUN pip3 install flask
```

## 4. imageのビルド
prototype_p001の部分は、新しいイメージの名前になるので、お好きな名前を。
:1.0の部分は、イメージのタグと呼ばれる部分で、主にバージョン名をつける。
バージョン指定なければ最新版になる
```
$ docker build . -t prototype_p001
>Successfully built 95a264abde1d
>Successfully tagged prototype_p001
```


5. コンテナ立ち上げ
$ docker run -it prototype_p001 
> root@8ef14f7b00d3:/#
で、立ち上げ＆ログイン完了

5.1 コンテナ内で色々やる

6. 再ログイン
$ docker ps -a
> NAMESを確認
$ docker start zealous_napier
$ docker exec -it zealous_napier /bin/bash

## 5. webアプリの作成
$ cd ~/Develop/docker/DockerPython3.6
$ mkdir vol
$ vi app.py
```
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return "Hello Words!!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

## 6.1 ディレクトリ構成
```
└─[$]> tree ./
./
├── Dockerfile
└── vol
    └── app.py
```

## 7 フォルダのマウント
$ docker run --name p3.6 -it -p 5000:5000 -v $(pwd)/vol:/home prototype_p001 /bin/bash

$ docker start p3.6
$ docker exec -it p3.6 /bin/bash
-vの部分：を挟んだ左側がホスト側のフォルダ、右側がコンテナ内のフォルダを指定しています。
コンテナにログインして、マウントしたフォルダに移動して色々確認。

## 8. アプリ起動
$ docker start python3.6
$ docker exec -it python3.6 /bin/bash
$ cd /home
$ python3 app.py

ブラウザで
http://localhost:5000/

Postman でも確認する。



