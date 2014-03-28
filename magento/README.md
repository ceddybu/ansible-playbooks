# magento ansible playbook
The goal of this playbook is to support deploying and configuring the entire magento stack on either bare metal dedicated servers or the Rackspace Cloud. 

# Requirements
So far this playbook only works on RHEL 6 and it's derivatives such as [CentOS 6](http://www.centos.org/) & [Scientific Linux 6](https://www.scientificlinux.org/).

# Vagrantfile
The included [Vagrantfile](https://docs.vagrantup.com/v2/vagrantfile/) uses [VagrantCloud](https://vagrantcloud.com/) box `chef/centos-6.5`. Easily deploy the stack on your local machine using [Virtualbox](https://www.virtualbox.org/).

## Tips
Vagrant will generate an ansible inventory file at `~/ansible-playbooks/magento/.vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory`


If you do use the Vagrantfile for running this playbook, running standalone `ansible` or `ansible-playbook` require a few extra arguments.


```bash
(user@host:~/ansible-playbooks/magento)$ ansible -u vagrant db -m setup -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory --private-key=~/.vagrant.d/insecure_private_key
```
# TODO
- php-fpm service on socket
- log rotation
- configure local.xml to use redis/memcached for the backend and session cache 
- memcached instances for legacy magento versions
- apache support w/ php-fpm
- supporting CE or EE
- Spinning up cloud servers behind a cloud load balancer (Rackspace)
