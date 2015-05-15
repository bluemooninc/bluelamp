FROM centos:centos6
MAINTAINER Yoshi Sakai <info@bluemooninc.jp>

#System Update & Install packages
RUN yum -y update

# install package
RUN yum -y install vim git
RUN yum -y install passwd openssh openssh-server openssh-clients sudo

# Add the ngix and PHP dependent repository
ADD nginx.repo /etc/yum.repos.d/nginx.repo

# Installing nginx 
RUN yum -y install nginx

# Installing PHP
RUN yum -y --enablerepo=remi,remi-php56 install nginx php-fpm  php-mbstring php-common php-mysql

# Adding the configuration file of the nginx
ADD default.nginx.conf /etc/nginx/conf.d/default.conf
ADD nginx.conf /etc/nginx/nginx.conf
ADD default.conf /etc/nginx/conf.d/default.conf
ADD www.conf /etc/php-fpm.d/www.conf
RUN mkdir /var/lib/php/session
RUN chmod 777 /var/lib/php/session

# Installing MySQL
# Add the MySql dependent repository
RUN yum install -y http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
RUN yum install -y mysql mysql-devel mysql-server  mysql-utilities

# Create user
RUN echo 'root:docker' | chpasswd
RUN useradd docker
RUN echo 'docker:docker' | chpasswd
RUN echo "docker    ALL=(ALL)       ALL" >> /etc/sudoers.d/docker

# Set up SSH
RUN mkdir -p /home/docker/.ssh; chown docker /home/docker/.ssh; chmod 700 /home/docker/.ssh
ADD id_rsa.pub /home/docker/.ssh/authorized_keys

RUN chown docker /home/docker/.ssh/authorized_keys
RUN chmod 600 /home/docker/.ssh/authorized_keys

# setup sudoers
RUN echo "docker ALL=(ALL) ALL" >> /etc/sudoers.d/docker

# Set up SSHD config
RUN sed -ri 's/#PermitRootLogin yes/PermitRootLogin yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config

# Init SSHD
RUN /etc/init.d/sshd start
RUN /etc/init.d/sshd stop

# Setup Mysql
RUN service mysqld start && \
    /usr/bin/mysqladmin -u root password "root"

# Set root path with default.nginx.conf share the local foler in vagrant
RUN mkdir /vagrant
RUN mkdir /vagrant/html
RUN ln -s /vagrant/html /var/www

######################################
#  Supervisord  ########################################

RUN yum -y install python-setuptools
RUN easy_install pip
RUN easy_install supervisor

ADD supervisord.conf /etc/supervisord.conf

#####################################
# Docker config #########################################

# Set the port to 22 80 3306
EXPOSE 22 80 3306

# run service by supervisord
CMD ["supervisord","-n"]
