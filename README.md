# apachehere

> Start Apache, Like A B(uiltin)S(erver).

好きなディレクトリで`apachehere`とタイプすると、そこをDocumentRootとしてApacheが立ち上がります。PHPのbuiltin serverみたいで手軽です。

phpenvがインストール済みなら、そのphpenvのphpをつかいます。phpenvのlocal指定も反映されます。mod_phpも用意すれば使えます（後述）。

OSX El Capitanでのみテストしていますが、おそらく他でも多少の手直しで動くと思われます。

## DISCLAIMER

LTで発表するために急いででっちあげましたので、完璧ではないかもしれません。

サンプルの`httpd.conf`はあくまで手元での開発用であり、最低限のオプションしかつけていません。Production用としては参考にしないほうが良いでしょう。

## インストール

### el capitanの環境

```
$ git clone this_repo

$ cp conf/httpd.conf.elcap conf/httpd.conf

# あとは `bin/apachehere` にパスを通して下さい。
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

```
$ APACHE_HTTPD_CONF_FILE="/Users/uzulla/dev/apachehere/conf/httpd.conf.elcap" \
./apachehere

$ APACHE_ORIG_SERVER_ROOT="/Users/uzulla/bin/apache" \
APACHE_ORIG_HTTPD="/Users/uzulla/bin/apache/bin/httpd" \
APACHE_HTTPD_CONF_FILE="/Users/uzulla/dev/apachehere/conf/httpd.conf.sample" \
./apachehere
```


## 使い方

### start

```
$ cd /path/to/your/htdocs
$ apachehere
# デフォルトでは http://127.0.0.1:8080/ になります
```

### stop

`^c`(Ctrl-C)を入力してください


## オプション

```
$ apachehere -h
Usage: apachehere [-t document_root]
[-b bind_ip] : default 127.0.0.1
[-p port_num] : default 8080
[-m] : use mod_php
[-c /path/to/php.ini]
[-s /path/to/php/conf.d]
```

- ポート80（など、1024以下）で動かす場合には、`sudo apachehere`としてください。USERはsudo前のものを指定します。

## `mod_php`について

`php-cgi`が許せない場合は、勿論`mod_php`がつかえます。

`~/.phpenv/versions/x.x.x/libexec/libphp7.so`などにファイルを用意した上で、`-m`オプションを指定してください。

`libphp5.so`や`libphp7.so`の作り方は「libphp7.so phpenv ビルド」とかでググるとよいでしょう。


## system phpの利用について

自分が使わないので積極的に対応していませんが、`/etc/apache2/`を参考に`httpd.conf`を修正すれば動くかも。

> FYI: El CapitanのSystem PHPに`php-cgi`はない


## これは本当に使い物になるの？

自分では使っていますが、普段はEl capitanのSystem Apacheはつかっていません。

System Apacheがどういうデフォルトオプションなのか完全には把握していないので、ビルド大好きな人は野良ビルドしたApacheを使うとなおよいでしょう。


## まとめ

`httpd.conf`に環境変数でパラメタをパスすれば色々便利ということが一番お伝えしたいことです。


