# Magento Ansible playbook
The goal of this playbook is to support deploying and configuring the entire magento stack on bare metal dedicated servers, purely cloud servers, or a hybrid environment.

## Requirements
An [EL 6](http://en.wikipedia.org/wiki/Category:Enterprise_Linux_distributions) derived operating system such as [CentOS 6](http://www.centos.org/) & [Scientific Linux 6](https://www.scientificlinux.org/).

## Supported Stacks

### Webservers 
- [nginx](http://wiki.nginx.org) + [php-fpm](http://php-fpm.org/)
- (todo) [apache](http://httpd.apache.org/) + [php-fpm](http://php-fpm.org/)

### PHP via IUS
- php 5.3
- php 5.4

#### Opcode Caching
- APC
- (todo) Zend OpCache

### Database
MySQL 5.5 flavors:
- [MariaDB](https://mariadb.org/)
- [Percona](http://www.percona.com/)
- [MySQL 5.5](http://www.mysql.com/) via [IUS](https://iuscommunity.org/)

### Caching Layer
- [redis](http://redis.io/)
- [memcached](http://memcached.org/)
- [varnish](https://www.varnish-cache.org/)

## Vagrantfile
The included [Vagrantfile](https://docs.vagrantup.com/v2/vagrantfile/) uses [VagrantCloud](https://vagrantcloud.com/) community box `chef/centos-6.5` or `puppetlabs/centos-6.5-64-nocm`. Easily deploy the stack on local [Virtualbox](https://www.virtualbox.org/) VMs.

### Tips
Vagrant will generate an ansible inventory file at `~/ansible-playbooks/magento/.vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory`.

Use `ssh-add ~/.vagrant.d/insecure_private_key` to add the vagrant key and simplify the standalone ansible commands. 

## TODO
- Deploy 3 redis instances instead of 1
- Deploy 1 memcached instance
- setup ntp in the common role
- dynamic, role based iptables management with ferm
- varnish + apache (2.4?) + php-fpm
- OPcode Caching
  - Investigate further APC tuning....
  - Install Zend OpCache via PECL. - php 5.3 and php 5.4 - opcache.php
- Create /etc/hosts files on each host referencing the other machines in the environment
- creating a separate, restricted nginx/php-fpm vhost for the magento admin panel backend
- spin up rackspace cloud servers behind cloud load balancers
  - private networks
  - hook into cloud monitoring
  - somehow integrate with heat/autoscale
- log rotation
- configure local.xml to use redis/memcached appropriately
- solr instance or elasticsearch single node/cluster, magento supports these as back-ends for full-text searches. 
- Install Magento CE or EE
- compatibility with debian / ubuntu

#### Notes
```
git clone https://github.com/samm-git/cm_redis_tools.git

rediscli.php supports command-line execution from crontab.
rediscli.php syntax:
Usage: rediscli.php <args>
-s <server> - server address
-p <port> - server port
-v - show process status
-d <databases> - list of the databases, comma separated
Example: rediscli.php -s 127.0.0.1 -p 6379 -d 0,1

Sample cron entry:
15 2 * * * /usr/bin/php /root/cm_redis_tools/rediscli.php -s 127.0.0.1 -p 6379 -d 0,1,2

**********
Using caches?

mysql> SELECT * FROM core_cache_option;
```
