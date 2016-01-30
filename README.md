# apachehere

> Start Apache, Like A B(uiltin)S(erver).

好きなディレクトリで`apachehere`とタイプすると、そこをDocumentRootとしてApacheが立ち上がります。PHPのbuiltin serverみたいで手軽です。

OSX El capitanならcloneしてpathを通すだけでSystemのApacheとPHPを利用した環境が立ち上がります。

phpenvがインストール済みならば、そちらのphpをつかいます。phpenvのlocal指定も反映されます。mod_phpも使えます（後述）。


## DISCLAIMER

OSX El Capitanでのみテストしていますが、おそらく他でも多少の手直しで動くと思われます。

サンプルの`httpd.conf`はあくまで手元での開発用であり、最低限のオプションしかつけていません。Production用としては参考にしないほうが良いでしょう。

## インストール

### El capitanの環境

cloneしてpathを通すだけです。 `phpenv`がなければ、System ApacheとSystem PHPを利用します。

```
$ git clone https://github.com/uzulla/apachehere.git
# `apachehere/bin`にpathを通すか、pathの通った所にapachehereをsymlinkする
```

System Apacheを使わない場合や、`httpd.conf`を修正したい場合は次を参照

### それ以外の環境、あるいは野良ビルドしたApacheを使う場合

```
$ git clone https://github.com/uzulla/apachehere.git

# sampleを元にhttpd.confを適当に作成
$ cp conf/httpd.conf.sample conf/httpd.conf
$ vi conf/httpd.conf

# 野良ビルドしたApacheにあわせて、
# `apachehere`コマンドの先頭あたりに以下を追記
# APACHE_ORIG_SERVER_ROOT="/Users/uzulla/apache"
# APACHE_ORIG_HTTPD="/Users/uzulla/apache/bin/httpd"
# ※ あるいは、実行時に毎回環境変数を設定
$ vi bin/apachehere

# `apachehere/bin`にpathを通すか、pathの通った所にapachehereをsymlinkする
```

### さらにあるいは、一部を環境変数で設定

プロジェクト毎に`httpd.conf`の設定が異なる場合には環境変数で`httpd.conf`の位置を指定できます。

```
$ APACHE_HTTPD_CONF_FILE="/path/to/your_project/nice_httpd_conf.conf" \
./apachehere

$ APACHE_ORIG_SERVER_ROOT="/path/to/your/apache" \
APACHE_ORIG_HTTPD="/path/to/your/apache/bin/httpd" \
APACHE_HTTPD_CONF_FILE="/path/to/your_project/nice_httpd_conf.conf" \
./apachehere
```

上記は1行で実行していますが、事前にexportでもよい。

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

