# [ServerSpec](https://serverspec.org/) 実行メモ
### ※Rubyがインストールされていることが前提条件
- ServerSpec用のフォルダを作成＆移動
```sh
$ mkdir serverspec_dir && cd $_
```
- ServerSpecをインストール
```sh
$ gem install serverspec
```
- 設定ファイルを生成
```sh
$ serverspec-init
Select OS type:

  1) UN*X
  2) Windows

Select number: 1  # amazon linux 2

Select a backend type:

  1) SSH
  2) Exec (local)

Select number: 2  # local

 + spec/
 + spec/localhost/
 + spec/localhost/sample_spec.rb
 + spec/spec_helper.rb
 + Rakefile
 + .rspec
```
- sample_spec.rb を試行するテスト内容に編集
```sh
$ nano spec/localhost/sample_spec.rb
```
- 実行
```sh
$ rake spec
```
