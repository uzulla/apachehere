# apachehere

> Start Apache, Like A B(uiltin)S(erver).

好きなディレクトリで`apachehere`とタイプすると、そこをDocumentRootとしてApacheが立ち上がります。PHPのbuiltin serverみたいで手軽です。

OSX El capitanならcloneしてpathを通すだけでSystemのApacheとPHPで動かす事ができます。

phpenvがインストール済みならそちらのphpをつかいます。phpenvのlocal指定も反映されます。mod_phpも用意すれば使えます（後述）。


## DISCLAIMER

OSX El Capitanでのみテストしていますが、おそらく他でも多少の手直しで動くと思われます。

サンプルの`httpd.conf`はあくまで手元での開発用であり、最低限のオプションしかつけていません。Production用としては参考にしないほうが良いでしょう。

## インストール

### El capitanの環境

cloneしてpathを通すだけです。 phpenvなどがなければ、System ApacheとSystem PHPを利用します。

```
$ git clone this_repo
# `bin/apachehere` にパスを通す

```

### それ以外の環境、あるいは野良ビルドしたApacheを使う場合

```
$ git clone this_repo

# sampleを元にhttpd.confを適当につくってください
$ cp conf/httpd.conf.sample conf/httpd.conf
$ vi conf/httpd.conf

# 野良ビルドしたApacheにあわせて、
# `apachehere`コマンドの先頭あたりに以下を追記
# APACHE_ORIG_SERVER_ROOT="/Users/uzulla/apache"
# APACHE_ORIG_HTTPD="/Users/uzulla/apache/bin/httpd"
$ vi bin/apachehere

# あとは `bin/apachehere` にパスを通して下さい。
```

### さらにあるいは、一部を環境変数で設定

プロジェクト毎に`httpd.conf`の設定が異なる場合には環境変数でhttpd.confの位置を指定できます。

```
$ APACHE_HTTPD_CONF_FILE="/path/to/your_project/nice_httpd_conf.conf" \
./apachehere

$ APACHE_ORIG_SERVER_ROOT="/path/to/your/apache" \
APACHE_ORIG_HTTPD="/path/to/your/apache/bin/httpd" \
APACHE_HTTPD_CONF_FILE="/path/to/your_project/nice_httpd_conf.conf" \
./apachehere
```

上記は1行で実行させていますが、勿論それぞれexportしてもよいでしょう。

## 使い方

### start

```
$ cd /path/to/your/htdocs
$ apachehere
# デフォルトでは http://127.0.0.1:8080/ になります
```

### stop

`^c`(Ctrl-C)を入力してください

- Apacheの終了に時間が掛かることがあります
- 盛大にエラーが出る事があります（実害ないとおもっていますが、もし良い解決策があれば知りたい…）

## オプション

```
$ apachehere -h
Usage: apachehere [-t document_root]
[-b bind_ip] : default 127.0.0.1
[-p port_num] : default 8080
[-c /path/to/php.ini]
[-s /path/to/php/conf.d]
```

- ポート80（など、1024以下）で動かす場合には、`sudo apachehere`としてください。USERはsudo前のものが利用されます。


## `mod_php`の用意について

`~/.phpenv/versions/x.x.x/libexec/libphp7.so`(あるいはlibphp5.so)にファイルを用意してください。自動的にそちらが使われます。

`libphp5.so`や`libphp7.so`の作り方は「libphp7.so phpenv ビルド」とかでググるとよいでしょう。

