# カワウソと学ぶネットワークプログラミング入門
セキュリティ・キャンプ 全国大会 2024で実施される講義中で使用する演習のためのファイルを配布するためのリポジトリです。
本講義では、演習にDockerコンテナを用います。

## 使い方

Dockerコンテナは、次のコマンドで起動できます。

```
$ docker compose up -d
```

開発用に用いるコンテナには、次のコマンドでログインできます。
sudoコマンドを実行した時に求められるパスワードは、`seccamp`です。

```
$ docker exec -it seccamp-pentester /bin/bash
```

## 各コンテナの情報

|概要 |IPアドレス                         |
|:--        |:--                        |
|BINDサーバ |10.8.9.2 |
|PostgreSQLサーバ |10.8.9.3 |
|SSHサーバ |10.8.9.4 |
|開発用コンテナ |10.8.9.5 |


## 謝辞
次のリポジトリを演習環境で使用させていただいております。
この場を借りてお礼申し上げます。

- https://github.com/knqyf263/CVE-2020-8617
