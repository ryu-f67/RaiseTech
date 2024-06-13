# 第5回課題メモ
## 組み込みサーバーでデプロイ
- EC2に接続
- sudo yum updateを実行
```sh
$ sudo yum update -y
```
- Gitのインストール
```sh
$ sudo yum install git
```
- rbenvをインストール
```sh
$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
$ source ~/.bash_profile
```
- rbenvのインストールを確認
```sh
$ rbenv -v
```
- ruby-buildのインストール
```sh
$ git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
$ cd ~/.rbenv/plugins/ruby-build
$ sudo ./install.sh
$ cd ..
```
- rubyに必要なパッケージをインストール
```sh
$ sudo yum -y install gcc-c++ glibc-headers openssl-devel readline libyaml-devel readline-devel zlib zlib-devel libffi-devel libxml2 libxslt libxml2-devel libxslt-devel sqlite-devel
```
- rbenvでバージョン(x.x.x)を指定してRubyをインストール
```sh
$ rbenv install x.x.x
$ rbenv global x.x.x
$ ruby -v
```
- Bundlerをバージョン(x.x.x)を指定してインストール
```sh
$ gem install bundler -v x.x.x
$ bundler -v
```
Railsをバージョン(x.x.x)を指定してインストール
```sh
$ gem install rails -v x.x.x
$ rails -v
```
- Node Version Manager (NVM) をインストール
```sh
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
$ source ~/.bashrc
```
- Node.jsをバージョン(x.x.x)を指定してインストール
```sh
$ nvm install x.x.x
$ node -v
```
- yarnをバージョン(x.x.x)を指定してインストール
```sh
# npmはnvmインストール時にインストールされている
$ npm install -global yarn@x.x.x
$ yarn -v
```
- サンプルアプリケーションをクローン
```sh
$ git clone "sample_application"
```
- インスタンス作成初期からインストールされているMariaDB用パッケージを削除する
```sh
# mariaDBを削除しておかないとmysql動作時エラーが生じる
$ sudo yum remove -y mariadb-*
```
- MySQLのリポジトリをyumに追加
```sh
$ sudo yum localinstall -y https://dev.mysql.com/get/mysql80-community-release-el7-11.noarch.rpm
```
- MySQLに必要なパッケージを取得
```sh
$ sudo yum install -y --enablerepo=mysql80-community mysql-community-server
$ sudo yum install -y --enablerepo=mysql80-community mysql-community-devel
```
- インストールされたMySQLに関係のあるパッケージを確認
```sh
$ yum list installed | grep mysql
```
- mysqlのlogファイルを作成
```sh
$ sudo touch /var/log/mysqld.log
```
- mysqlを起動
```sh
$ sudo systemctl start mysqld
```
- mysqlの状態を確認
```sh
$ systemctl status mysqld.service
```
- mysqlがインスタンスの起動と同時に起動するように設定
```sh
$ sudo systemctl enable mysqld
```
- mysqlの初期パスワードの確認
```sh
$ sudo cat /var/log/mysqld.log | grep "temporary password"
```
- ログイン確認
```sh
$ mysql -u root -p
Enter password: パスワードを入力 # パスワードは入力しても表示されない
```
- mysqlのパスワードを変更する
```sh
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '新しいパスワード';
mysql> FLUSH PRIVILEGES;
# exit or ctrl+Dでmysqlから抜ける
```
- サンプルアプリへディレクトリの移動
```sh
$ cd ./sample_application
```
- データベースの編集
```sh
$ cp config/database.yml.sample config/database.yml
$ nano config/database.yml
```
- database.ymlに追加、または、編集
```sh
host: RDSのエンドポイント
port: RDSのポート
username: RDSのユーザ－ネ－ム
password: RDSのパスワード
```
- 環境構築
```sh
$ bin/setup
```
- 組み込みサーバーを走らせる
```sh
$ bin/dev
```
