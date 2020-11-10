docker-mirakurun-epgstation
====

[Mirakurun](https://github.com/Chinachu/Mirakurun) + [EPGStation](https://github.com/l3tnun/EPGStation) の Docker コンテナ

## 前提条件

Docker, docker-compose の導入が必須

PT3 + [m-tsudo/pt3](https://github.com/m-tsudo/pt3) 、PX-Q3PE4、BonDriverProxyClientなど、複数の組み合わせを想定しているため
recpt1/recbond/libaribb25 を導入する

ホスト上の pcscd は停止する

## インストール手順

```
$ git clone https://github.com/ykym/docker-mirakurun-epgstation.git
$ cd docker-mirakurun-epgstation

# libairbb25系は派生が多いため気に入ったものがあれば以下のディレクトリに配置
ex) $ git clone https://github.com/epgdatacapbon/libaribb25 ./mirakurun/libaribb25

$ sudo docker-compose pull
$ sudo docker-compose build

# チャンネル設定
$ vim mirakurun/conf/channels.yml
# Mirakurun のチャンネルスキャンを使う場合(GR)
$ curl -X PUT "http://localhost:40772/api/config/channels/scan"

# コメントアウトされている restart や user の設定を適宜変更する
$ vim docker-compose.yml
```

## 起動

```
$ sudo docker-compose up -d
```
mirakurun の EPG 更新を待ってからブラウザで http://DockerHostIP:8888 へアクセスし動作を確認する

## 停止

```
$ sudo docker-compose down
```

## 設定

### Mirakurun

* ポート番号: 40772

### EPGStation

* ポート番号: 8888

### 各種ファイル保存先

* 録画データ

```./recorded```

* サムネイル

```./epgstation/thumbnail```

* 予約情報と HLS 配信時の一時ファイル

```./epgstation/data```

* EPGStation 設定ファイル

```./epgstation/config```

* EPGStation のログ

```./epgstation/logs```
