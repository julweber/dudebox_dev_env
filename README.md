# Description

This repo serves as a starting point to manage a general development environment for creating ruby, java, nodejs and go applications. It provides service installations to be accessed by your applications.

# Services

The current version installs the following services on the VM:
* PostgreSQL - [Documentation](http://www.postgresql.org/docs/)
* Mongodb - [Documentation](http://docs.mongodb.org/manual/)
* Redis - [Documentation](http://redis.io/commands)
* RabbitMQ - [Documentation](https://www.rabbitmq.com/documentation.html)

All services can be accessed from the host system using their default ports and the host 127.0.0.1. For a list of forwarded ports please refer to the [Vagrantfile](Vagrantfile).

For a general description about database technologies and choice please have a look [here](https://www.digitalocean.com/community/tutorials/understanding-sql-and-nosql-databases-and-different-database-models).

!!! And please never use Redis to store persistent data. Redis should be used only as a cache !!!

# Prerequisites

* [Install Virtualbox](https://www.virtualbox.org/wiki/Downloads) **or** [install VmWare](http://www.vmware.com/de/products/player)
* [Install Vagrant](https://www.vagrantup.com/downloads.html)


## !!! Install vagrant plugins !!!

The Chef provisioner will only run correctly when the vagrant-librarian-chef-nochef plugin is installed correctly.

    vagrant plugin install vagrant-vbguest
    vagrant plugin install vagrant-librarian-chef-nochef

# Startup

    # clone the repo
    git clone https://github.com/julweber/dudebox_dev_env.gits

    # enter the directory
    cd dudebox_dev_env

    # start the vagrant machine
    vagrant up

    # ssh into the machine when the provisioning process has finished
    vagrant ssh

# Reprovisioning

When making changes to the Cheffile and Vagrantfile please reprovision the VM using the following commands:

    vagrant up
    vagrant provision

# Shared folders

The workspace folder is mounted to the VM under /vagrant/workspace.
Put your files (sources, ...) in this folder to be able to access the files from your guest and host systems.

# Default Service Authentication

* PostgreSQL: User: postgres ; password: test123!
* MongoDB: auth disabled -> all connections accepted
* Redis: auth disabled -> all connections accepted
* RabbitMQ: User: guest ; password: guest

# Troubleshooting

## Errors on reprovisioning

Error message: "Shared folders that chef requires are missing ...."

    rm .vagrant/machines/default/virtualbox/synced_folders
    vagrant reload --provision

## Operating behind a Proxy

### Install the vagrant-proxyconf plugin

    $> vagrant plugin install vagrant-proxyconf

### Add the proxy configuration to your Vagrantfile

    if Vagrant.has_plugin?("vagrant-proxyconf")
      config.proxy.http     = "http://192.168.0.2:3128/" # exchange correct http proxy here
      config.proxy.https    = "http://192.168.0.2:3128/" # exchange correct https proxy here
      config.proxy.no_proxy = "localhost,127.0.0.1,.example.com"
    end

## Windows Command log_filename

Please execute the commands above in a command line instance with administrator rights.

## SSH into the machine without using the vagrant client

You can use a normal ssh client (command line, putty, ...) to ssh into the virtual machine. The user and password is vagrant.

    ssh -p 2222  vagrant@127.0.0.1 # pw: vagrant
