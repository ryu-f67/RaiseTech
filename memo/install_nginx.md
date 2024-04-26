# 第5回課題メモ
## Nginxを起動
- EC2に接続
- sudo yum updateを実行
```sh
$ sudo yum update -y
```
- インストールできるものを確認(nginx1)
```sh
$ sudo amazon-linux-extras
  2  httpd_modules            available    [ =1.0  =stable ]
  3  memcached1.5             available    \
        [ =1.5.1  =1.5.16  =1.5.17 ]
  9  R3.4                     available    [ =3.4.3  =stable ]
 10  rust1                    available    \
        [ =1.22.1  =1.26.0  =1.26.1  =1.27.2  =1.31.0  =1.38.0
          =stable ]
 18  libreoffice              available    \
        [ =5.0.6.2_15  =5.3.6.1  =stable ]
 19  gimp                     available    [ =2.8.22 ]
 20 †docker=latest            enabled      \
        [ =17.12.1  =18.03.1  =18.06.1  =18.09.9  =stable ]
 21  mate-desktop1.x          available    \
        [ =1.19.0  =1.20.0  =stable ]
 22  GraphicsMagick1.3        available    \
        [ =1.3.29  =1.3.32  =1.3.34  =stable ]
 24  epel                     available    [ =7.11  =stable ]
 25  testing                  available    [ =1.0  =stable ]
 26  ecs                      available    [ =stable ]
 27 †corretto8                available    \
        [ =1.8.0_192  =1.8.0_202  =1.8.0_212  =1.8.0_222  =1.8.0_232
          =1.8.0_242  =stable ]
 32  lustre2.10               available    \
        [ =2.10.5  =2.10.8  =stable ]
 33 †java-openjdk11           available    [ =11  =stable ]
 34  lynis                    available    [ =stable ]
 36  BCC                      available    [ =0.x  =stable ]
 37  mono                     available    [ =5.x  =stable ]
 38  nginx1                   available    [ =stable ]
 40  mock                     available    [ =stable ]
 43  livepatch                available    [ =stable ]
 44 †python3.8                available    [ =stable ]
 45  haproxy2                 available    [ =stable ]
 46  collectd                 available    [ =stable ]
 47  aws-nitro-enclaves-cli   available    [ =stable ]
 48  R4                       available    [ =stable ]
  _  kernel-5.4               available    [ =stable ]
 50  selinux-ng               available    [ =stable ]
 52  tomcat9                  available    [ =stable ]
 53  unbound1.13              available    [ =stable ]
 54 †mariadb10.5              available    [ =stable ]
 55  kernel-5.10=latest       enabled      [ =stable ]
 56  redis6                   available    [ =stable ]
 58 †postgresql12             available    [ =stable ]
 59 †postgresql13             available    [ =stable ]
 60  mock2                    available    [ =stable ]
 61  dnsmasq2.85              available    [ =stable ]
 62  kernel-5.15              available    [ =stable ]
 63 †postgresql14             available    [ =stable ]
 64  firefox                  available    [ =stable ]
 65  lustre                   available    [ =stable ]
 66 †php8.1                   available    [ =stable ]
 67  awscli1                  available    [ =stable ]
 68 †php8.2                   available    [ =stable ]
 69  dnsmasq                  available    [ =stable ]
 70  unbound1.17              available    [ =stable ]
 72  collectd-python3         available    [ =stable ]
† Note on end-of-support. Use 'info' subcommand.
```
- nginxをインストール
```sh
$ sudo amazon-linux-extras install nginx1
```
- nginxを起動
```sh
$ sudo systemctl start nginx
```
- nginxの状態確認
```sh
$ sudo systemctl status nginx
```
- nginxの起動を確認(EC2のパブリックIPでブラウザにアクセス)
![Nginx起動確認](images/memo_imgs/startNginx.png)


### Nginxのコマンドまとめ
- 起動
```sh
$ sudo systemctl start nginx
```
- 停止
```sh
$ sudo systemctl stop nginx
```
- 再起動
```sh
$ sudo systemctl restart nginx
```
- 状態確認
```sh
$ sudo systemctl status nginx
```
- スタ－トアップ登録
```sh
$ sudo systemctl enable nginx
```
- スタ－トアップ解除
```sh
$ sudo systemctl disable nginx
```
- 設定ファイルのチェック
```sh
$ nginx -t
```
- Nginxの基本設定ファイル
```sh
$ sudo cat /etc/nginx/nginx.conf
```
- デフォルト時のエラーログの場所
```sh
/var/log/nginx/error.log
```
- デフォルト時のアクセスログの場所
```sh
/var/log/nginx/access.log
```
 - アクセスログの内容は設定ファイルのログフォーマットに記載された項目が表示される
 ![Nginxのログフォーマット](images/memo_imgs/Nginx_setup_file.png)