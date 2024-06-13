# 第5回課題メモ
## Puma + Nginx でデプロイ
- [EC2deploy.md](/memo/EC2deploy.md) と [install_nginx.md](/memo/install_nginx.md) を参考に必要な工程まで実施
- systemdで起動するため puma.service を /etc/systemd/system の下に作成
- nginx.conf を編集
  - userをnginx から ec2-user に変更
  ```sh
  user ec2-user; # 変更前は user nginx;
  ```
  - server_nameにEC2のパブリックIPを指定
  - httpモジュールに下記を追加
  ```sh
  upstream app_server {
  server unix:/home/ec2-user/app/tmp/sockets/puma.sock;
  }
  ```
  - rootにappのpublicのパスを指定
  ```sh
   root  /home/ec2-user/app/public;
  ```
  - server内に下記を追加
  ```sh
  location ~ ^/assets/ {
    gzip_static on;
    expires max;
    add_header Cache-Control public;
  }

  try_files $uri/index.html $uri @app_server;

  location @app_server {
    proxy_pass http://app_server;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
  }
  ```
- nginx.confの構文にエラーがないかチェックする
```sh
$ sudo nginx -t
```
- nginx.confにエラーがなかったら、Nginxを起動する
```sh
$ sudo systemctl start nginx
```
- puma.rb を編集
  - directryを指定(appのディレクトリのパス)
  - portの指定を外す(コメントアウト)
- development.rbに下記を追加
```sh
config.web_console.allowed_ips = '0.0.0.0/0'
```
- assetsのコンパイル
```sh
$ bundle exec rails assets:precompile RAILS_ENV=development
```
- Pumaを起動
```sh
$ sudo systemctl start puma.service
```
- 実行時 pidやsocketに対して No such file or directory のエラ－が発生した場合、以下のどちらかを行う
  - tmpフォルダにsocketsフォルダとpidsフォルダを作成
  - puma.rbのsocketfileのパスとpidfileのパスを書き換える
- PumaとNginxが起動出来たらEC2のパブリックIPでブラウザにアクセス