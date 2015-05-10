# bluelamp
Vagrant Docker for LAMP ( nginx, PHP, MySQL )

## 第１章 Vagrant のインストール

### 1：公式サイトからダウンロードしてインストール
http://www.vagrantup.com/

### 2：ターミナルを起動
$home/dev フォルダなどVagrant専用のフォルダを作る

### 3：vagrant起動
>vagrant up

### 4：起動確認
>vagrant status

### 5:仮想マシンへの接続
>vagrant ssh

## 第２章 Dockerインストール
vagrant ssh でvagrant に入りdockerをインストールします。

### 1:インストール

sudo yum install -y docker-io

service docker status

sudo service docker start

管理サーバに公開鍵を作成しておきます。

[root@vagrant-centos6 docker]# ssh-keygen

[root@vagrant-centos6 docker]# cp ~/.ssh/rsa_id.pub ./

### 2:Dockerfileのbuild
cd /vagrant/docker

sudo docker build -t="bluemoon/lamp" .

docker run -i -t -d -p 8080:80 -p 2222:22 -p 3306:3306 --name moonlamp -v /vagrant:/vagrant:rw bluemoon/lamp

## 第３章　ローカルからアクセス

### 1:ブラウザから
http://192.168.33.10:8080/

### 2:ターミナルから
ssh -p 2222 docker@192.168.33.10
PW は docker

### 3:MySQLクライアントから
mysql:192.168.33.10:3306 root
pw は root
