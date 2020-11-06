---
layout: post
date:   2020-11-06 13:13:26 +0200
title:  "Magento2 with Vagrant"
category: magento2
tags: [magento2, vagrant]
---
`Vagrant` is a tool for building and managing virtual machine environments in a single workflow. <br />
`Oracle VM VirtualBox` is a free and open-source hosted hypervisor for x86 virtualization.

<br /><br />

<h2>Install Vagrant and Virtualbox</h2>
{% highlight javascript %}
sudo apt-get install virtualbox
curl -O https://releases.hashicorp.com/vagrant/2.2.9/vagrant_2.2.9_x86_64.deb
sudo apt install ./vagrant_2.2.9_x86_64.deb
{% endhighlight %}

<br /><br />
Now you need `Vagrantfile` (see below) inside directory when you want to run Vagrant. <br /><br />
`vagrant up` - creates and configures guest machines according to your Vagrantfile <br />
`vagrant destroy` - stops the running machine Vagrant is managing and destroys all resources that were created during the machine creation process <br />
`vagrant ssh` - SSH into a running Vagrant machine and give you access to a shell
<br /><br />
You should be able to see your project on `http://localhost`.



<br /><br />

<h2>Vagrantfile</h2>

<h3>Magento 2.4.1 with php7.3 and ElasticSearch</h3>

{% highlight javascript %}
# -*- mode: ruby -*-
# vi: set ft=ruby :
#
$site = <<-CONF
upstream fastcgi_backend {
  server  unix:/run/php/php7.3-fpm.sock;
}

server {
  listen 80;
  server_name _;
  set \\$MAGE_ROOT /var/www/html/;
  include /var/www/html/nginx.conf.sample;
}
CONF

$script = <<-SCRIPT
set -e

export DEBIAN_FRONTEND=noninteractive


wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list


add-apt-repository ppa:ondrej/php
apt-get update
apt-get install -y php7.3

apt update
apt upgrade -y

apt install -y\
	php7.3-fpm php7.3-bcmath php7.3-curl php7.3-dom php7.3-gd php7.3-intl php7.3-mbstring php7.3-cli\
	php7.3-simplexml php7.3-soap php7.3-xsl php7.3-zip php7.3-pdo-mysql\
	nginx composer git mysql-server

apt install -y elasticsearch

 service elasticsearch start

if [ -f "/var/www/html/index.nginx-debian.html" ]; then
	rm /var/www/html/index.nginx-debian.html
fi

if [ ! -f "/var/www/html/.git" ]; then
	rm /var/www/_html -rfv
	mkdir /var/www/_html
	chown www-data:www-data /var/www/*html
	sudo -u www-data git clone https://github.com/magento/magento2.git --depth 1 --single-branch --branch 2.4.1 /var/www/_html
	rsync -a /var/www/_html/ /var/www/html/
fi

echo "#{$site}" > /etc/nginx/sites-enabled/default

#stop apache2 for some reason apache2 was running and we want to use nginx
service apache2 stop
service nginx start

#composer 1.9.1 required for installing magento 2.4.1
sudo apt-get install curl
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer --version=1.9.1


cd /var/www/html

composer install

find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
chmod u+x bin/magento

echo "CREATE DATABASE IF NOT EXISTS magento" | mysql
echo "CREATE USER IF NOT EXISTS 'magento'@'localhost' IDENTIFIED BY 'magento';" | mysql
echo "GRANT ALL PRIVILEGES ON magento.* TO 'magento'@'localhost';" | mysql

if [ ! -f "/var/m2-data/.git" ]; then
	git clone https://github.com/magento/magento2-sample-data.git /var/m2-data
fi

sudo chmod -R 777 /var/www/html/var /var/www/html/pub /var/m2-data

php -f /var/m2-data/dev/tools/build-sample-data.php -- --ce-source="/var/www/html"

cd bin

./magento setup:install \
	--base-url=http://localhost:8080 \
	--db-host=localhost \
	--db-name=magento \
	--db-user=magento \
	--db-password=magento \
	--backend-frontname=admin \
	--admin-firstname=admin \
	--admin-lastname=admin \
	--admin-email=admin@admin.com \
	--admin-user=admin \
	--admin-password=admin123 \
	--language=en_US \
	--currency=USD \
	--timezone=UTC \
	--use-rewrites=1

./magento deploy:mode:set developer

SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.provision :shell, inline: $script
  
  #this is synced module, we cloned it before running vagrant into the dir
  config.vm.synced_folder "./", "/var/www/html/app/code/Gateway3D/PersonaliseIt", :mount_options => [ "ro" ]
  #config.vm.synced_folder "./", "/code", :mount_options => [ "ro" ]
  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024 * 4
  end
end
{% endhighlight %}


<br /><br />

<h3>Magento 2.3.4 with php7.2</h3>
{% highlight javascript %}
# -*- mode: ruby -*-
# vi: set ft=ruby :
#
$site = <<-CONF
upstream fastcgi_backend {
  server  unix:/run/php/php7.2-fpm.sock;
}

server {
  listen 80;
  server_name _;
  set \\$MAGE_ROOT /var/www/html/;
  include /var/www/html/nginx.conf.sample;
}
CONF

$script = <<-SCRIPT
set -e

export DEBIAN_FRONTEND=noninteractive


apt update
apt upgrade -y

apt install -y\
	php-fpm php-bcmath php-curl php-dom php-gd php-intl php-mbstring php-cli\
	php-simplexml php-soap php-xsl php-zip php-pdo-mysql\
	nginx composer git mysql-server

#apt install -y elasticsearch

if [ -f "/var/www/html/index.nginx-debian.html" ]; then
	rm /var/www/html/index.nginx-debian.html
fi

if [ ! -f "/var/www/html/.git" ]; then
	rm /var/www/_html -rfv
	mkdir /var/www/_html
	chown www-data:www-data /var/www/*html
	sudo -u www-data git clone https://github.com/magento/magento2.git --depth 1 --single-branch --branch 2.3.4 /var/www/_html
	rsync -a /var/www/_html/ /var/www/html/
fi

echo "#{$site}" > /etc/nginx/sites-enabled/default

#stop apache2 for some reason apache2 was running and we want to use nginx
service apache2 stop
service nginx start

cd /var/www/html

composer install

find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
chmod u+x bin/magento

echo "CREATE DATABASE IF NOT EXISTS magento" | mysql
echo "CREATE USER IF NOT EXISTS 'magento'@'localhost' IDENTIFIED BY 'magento';" | mysql
echo "GRANT ALL PRIVILEGES ON magento.* TO 'magento'@'localhost';" | mysql

if [ ! -f "/var/m2-data/.git" ]; then
	git clone https://github.com/magento/magento2-sample-data.git /var/m2-data
fi

sudo chmod -R 777 /var/www/html/var /var/www/html/pub /var/m2-data

php -f /var/m2-data/dev/tools/build-sample-data.php -- --ce-source="/var/www/html"

cd bin

./magento setup:install \
	--base-url=http://localhost:8080 \
	--db-host=localhost \
	--db-name=magento \
	--db-user=magento \
	--db-password=magento \
	--backend-frontname=admin \
	--admin-firstname=admin \
	--admin-lastname=admin \
	--admin-email=admin@admin.com \
	--admin-user=admin \
	--admin-password=admin123 \
	--language=en_US \
	--currency=USD \
	--timezone=UTC \
	--use-rewrites=1

./magento deploy:mode:set developer

SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.provision :shell, inline: $script
  #this is synced module, we cloned it before running vagrant into the dir
  config.vm.synced_folder "./", "/var/www/html/app/code/Gateway3D/PersonaliseIt", :mount_options => [ "ro" ]
  #config.vm.synced_folder "./", "/code", :mount_options => [ "ro" ]
  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024 * 4
  end
end
{% endhighlight %}

<br /><br />


