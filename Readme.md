# docker コンテナ環境の構築手順

## 1. 含まれるコンテナ

- mysql
  - DB本体
- phpMyAdmin
  - DB制御補助
- nodered
  - 各種サーバ制御
- superset
  - BIツール

## 2. ディレクトリ構造

|-- mount  
|   |-- db (mysql 関連のマウント先)  
|   |    |-- data (db 本体が格納される)  
|   |    |   |-- .gitignore (data ディレクトリをリポジトリに残すためだけに設置。)  
|   |    |-- my.cnf (文字コードなどを設定する)  
|   |    |-- sql (データベースの初期化関連)  
|   |         |-- 001-create-tables.sql (初期化時に実行するクエリを格納)  
|   |         |-- init-database.sh　(初期化処理のためのスクリプト)  
|   |-- nodered  
|   |    |-- settings.js   
|   |    |-- package.json  
|   |    |-- node_modules (mysql やemail など最低限のノードを格納)   
|   |    |-- (フローファイル、暗号化済み認証情報)  
|   |-- nginx  
|   |    |-- public  
|   |         |-- index.html  
|   |-- superset  
|-- docker-compose.yml  
|-- .env  
|-- Readme.md  
|-- dockerfile  
     |-- superset  
          |-- Dockerfile, etc...  

## 3. 構築手順

### 3.1. 環境変数の設定

.env を編集してください。

| カテゴリ   | 環境変数名          | 概要                                                    |
| ---------- | ------------------- | ------------------------------------------------------- |
| common     | CONTAINER_BASE_NAME | コンテナの頭につけるプロジェクト名                      |
| node-red   | NODE_RED_PORT       | コンテナ外からアクセスするときのnode-red のポート番号   |
| node-red   | NODE_RED_FLOW       | 最初にアクセスされるフローのフロー名                    |
|node-red|NODE_RED_CREDENTIAL_SECRET||
| mysql      | MYSQL_PORT          | コンテナ外からアクセスするときのmysql のポート番号      |
| mysql      | MYSQL_DATABASE      | 最初に生成するデータベース名                            |
| mysql      | MYSQL_ROOT_PASSWORD | root ユーザの初期パスワード                             |
| mysql      | MYSQL_USER          | 通常アクセスするときのためのユーザ名                    |
| mysql      | MYSQL_PASSWORD      | 上記ユーザ名に対する初期パスワード                      |
| nginx      | NGINX_PORT          | コンテナ外からアクセスするときのnginx のポート番号      |

### 3.2. コンテナ群の生成及び起動

```
sudo docker compose up -d
```

### 3.3. metabase のアカウント初期化

![metabase初期化画面](.\images\metabase000.png)

最初にアクセスすると、上記画面のように2 Add your data の画面で色々聞かれるので、以下の内容をそれぞれ入力する。

| 項目              | 概要                               |
| ----------------- | ---------------------------------- |
| (データベース)    | MySQL を選択する                   |
| Name              | 好きな名前を入力する               |
| Host              | db で固定                          |
| Port              | 3306 で固定                        |
| Database name     | 環境変数 MYSQL_DATABASE に合わせる |
| Database username | 環境変数MYSQL_USER に合わせる      |
| Database password | 環境変数MYSQL_PASSWORD に合わせる  |



## 4. 注意事項

### 4.1. NodeRED のmysql ノードからmysql database にアクセスする場合

ノードで設定するホスト名をdb, ポート番号を3306 にしておくこと。

ホスト名はlocalhost や127.0.0.1 ではダメ。db というのは、docker-compose.yml に記載したmysql のサービス名であり、docker コンテナをまたいでネットワークアクセスする時にホスト名として使用する。




## 5. 参考情報
- node-red & docker:
  - https://nodered.jp/docs/getting-started/docker




