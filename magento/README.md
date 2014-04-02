# magento ansible playbook
The goal of this playbook is to support deploying and configuring the entire magento stack on either bare metal dedicated servers or the Rackspace Cloud. 

# Requirements
So far this playbook only works on RHEL 6 and it's derivatives such as [CentOS 6](http://www.centos.org/) & [Scientific Linux 6](https://www.scientificlinux.org/).

# Vagrantfile
The included [Vagrantfile](https://docs.vagrantup.com/v2/vagrantfile/) uses [VagrantCloud](https://vagrantcloud.com/) box `chef/centos-6.5`. Easily deploy the stack on your local machine using [Virtualbox](https://www.virtualbox.org/).

## Tips
Vagrant will generate an ansible inventory file at `~/ansible-playbooks/magento/.vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory`.

Use `ssh-add ~/.vagrant.d/insecure_private_key` to add the vagrant key and simplify the standalone ansible commands. 

# TODO
- Investigate further APC tuning....
- Create /etc/hosts files on each host
- Config testing handlers (nginx -t - httpd -t)
- add php 5.3 support for the safety/stability crowd (including myself)
- dynamic, role based iptables management with ferm
- creating a separate, restricted nginx/php-fpm vhost for the magento admin panel backend
- spin up rackspace cloud servers behind cloud load balancers
  - private networks
  - hook into cloud monitoring
  - somehow integrate with heat/autoscale
- log rotation
- configure local.xml to use redis/memcached for the backend and session cache 
- memcached instances for legacy magento versions or noobs
- solr instance or elasticsearch single node/cluster, magento supports these as back-ends for full-text searches. 
- apache support w/ php-fpm
  - apache 2.4 ?
- supporting CE or EE (full page cache config mostly)
- compatibility with debian / ubuntu

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
