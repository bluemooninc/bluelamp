# bluelamp
Vagrant Docker for LAMP ( nginx, PHP, MySQL )

## 第１章 Vagrant のインストール

### 1：公式サイトからダウンロードしてインストール
以下、VirtualBoxとVagrantをそれぞれインストールします。VirtualBoxが古かったり新しすぎたりするとVagrantと相性が悪くなって動かない場合があります。動かない場合は組み合わせが悪いのでバージョンに注意しましょう。
https://www.virtualbox.org/
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
vagrant ssh でvagrant に入りdockerをインストールします。

1:インストール
EPELパッケージを先にインストールします。

rpm -iUvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

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

phpMyAdmin や WordPress などは /vagrant/html/ フォルダにローカルで配置します。
以下にアクセスすると動作します。

http://192.168.33.10:8080/phpMyAdmin
http://192.168.33.10:8080/wordpress

### 2:ターミナルから
ssh -p 2222 docker@192.168.33.10
PW は docker

### 3:MySQL Workbench / Aquafold 等から
192.168.33.10:3306 root
pw は root
