# docker_apache
[自分のdockerhub](https://hub.docker.com/r/satjopg/httpd/)の備忘録  
dockerhubよりgithubのがみるのでこっちに書く.

# 仕様
当たり前だけど, Dockerが動かせる環境で...  

* コンテナのベース:centos7
* apache 2.4.25

# 使い方

* ローカルに落とす

```console
$ docker pull satjopg/httpd:latest
```

* 稼働

```console
$ docker run --privileged -d -p (ホストのポート:apacheで開けているポート) --name (コンテナの名前) satjopg/httpd:latest /sbin/init
```

* コンテナの中に入る

```console
$ docker exec -it (コンテナの名前orコンテナID) /bin/bash
```

* (コンテナ内)サーバの稼働

```console
# systemctl start httpd
```

* (コンテナ内)サーバの状態

```console
# systemctl status httpd
```

* (コンテナ内)サーバの停止

```console
# systemctl stop httpd
```

* (コンテナ内)サーバの自動稼働

```console
# systemctl enable httpd.service
```

# 忘れそうなこと
## 稼働について

* apacheは解放されるポートはhttp.confで定義されている.  
(デフォルトは80番)

* ホスト, コンテナのポート対応を80番で同じにするとpowなどの他のものと被る恐れがあるので,注意.  
(空いているポートを選べばおけ)

* --privileged: 全てのデバイスに接続する権限を起動するコンテナに与える.
([SELinux](http://rfs.jp/server/security/selinux01.html)有効時はつける)

* いきなりbashを動かして中に入ると, 「Failed to get D-Bus connection: No connection to service manager.」というエラーが出るので注意

* (上記続き) これはcentos7ではinitデーモンがsystemdに変更になったためである.  
(/sbin/init/が, systemdのシンボリックリンクとなっているのでこいつを動かせばおけ.名前ややこしすぎるだろ.)

## コンテナ内操作

* centos7でのネットワーク関連は以下のパッケージをインストールする.  
これは, net-toolsが非推奨となったためである.  
(iproute2が入らんかったのでとりあえず, iproute載せとく)

```console
# yum -y install iproute
```

* IPアドレスなどの確認

```console
$ ip a
```
