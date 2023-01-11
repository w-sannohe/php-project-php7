# php-project-php7
php7.3の環境を整えました

## 環境
nginx ・・・ 1.23  
mysql ・・・ 8.0  
php   ・・・ 7.4.3  

## ビルドのやり方
` $ docker-compose up --build`

## 確認方法
web(Hello world!!が出たらok)  
http://localhost/

DB(phpMyAdminが表示されたらok)  
http://localhost:8083/index.php
(もしphpMyAdminが見れなかったら、mysql.sockのやつかも。こんなときは↓↓を見てみてください)

## こんなときは。。。

### DBをカスタマイズしたい
ビルドする前に、docker-compose.ymlの下記の場所を任意で変更してビルドする  
```
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root　　  ⇦ rootのパスワード
      MYSQL_DATABASE: php_project   ⇦ DBの名前
      MYSQL_USER: phper             ⇦ ログインuser名
      MYSQL_PASSWORD: phper_p       ⇦ ログインパスワード
    # command:
    #   mysqld --default-authentication-plugin=mysql_native_password
    volumes:
      - ./docker/mysql/data:/var/lib/mysql
      - ./docker/mysql/my.cnf:/etc/mysql/conf.d/my.cnf
      - ./logs:/var/log/mysql
    ports:
      - 33306:3306
```

### phpMyAdminが見れない
もしかしたら、mysql.sockのせいかも!?  
dokcer-compose upで立ち上げて、[phpMyAdminを表示](http://localhost:8083/index.php)、dockerのログからこうやって↓出てくるのを待ってみる  
```db_1          | 2023-01-11 04:33:57+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.```  
それが出てきたらもう一回見てみる( [phpMyAdminを表示](http://localhost:8083/index.php) )  

### timezoneでUTCとAsia/Tokyoで違うよっていうエラーが出た時
homebrewからインストールしたphpのphp.iniの設定不足かも!?  

PCのタイムゾーンを確認する  
`$ php -i | grep php.ini`

UTCと出たら、php.iniの場所を確認して  
`$ php -i | head`

```
$ php -i | head
phpinfo()
PHP Version => 7.4.30

Configuration File (php.ini) Path => /opt/homebrew/etc/php/7.4
Loaded Configuration File => /opt/homebrew/etc/php/7.4/php.ini　⇦ここがphp.iniの場所
```

php.iniを書き換える  

`$ sudo vi /opt/homebrew/etc/php/7.4/php.ini`

```
php.iniの下記の位置を「;」とって下記のように追記する

date.timezone = "Asia/Tokyo"
```
