# laravel-box
Vagrantbox for laravel, uses scotch/box with a few customizations in vagrant file

All credits for the used vagrant box inside goes to [Scotch Box](http://box.scotch.io/).
Check out the official docs at: [box.scotch.io](http://box.scotch.io).

## Features

### System Stuff

- Ubuntu 14.04 LTS (Trusty Tahr)
- PHP 7.0
- Ruby 2.2.x
- Vim
- Git
- cURL
- GD and Imagick
- Composer
- Beanstalkd
- Node
- NPM
- Mcrypt

### Database Stuff
- MySQL
- PostgreSQL
- SQLite

### Caching Stuff

- Redis
- Memcache and Memcached

### Node Stuff

- Grunt
- Bower
- Yeoman
- Gulp
- Browsersync
- PM2

### Laravel Stuff

- Laravel Installer
- Laravel Envoy
- Blackfire Profiler

### Other Useful Stuff

- No Internet connection required
- PHP Errors turned on
- Laravel and WordPress ready
- Operating System agnostic
- Goodbye XAMPP / WAMP
- New Vagrant version? Update worry free. ScotchBox is very reliable with a lesser chance of breaking with various updates
- Super easy database access and control
- [Virtual host ready](https://scotch.io/bar-talk/announcing-scotch-box-2-0-our-dead-simple-vagrant-lamp-stack-improved#multiple-domains-(virtual-hosts))
- PHP short tags turned on
- H5BP’s server configs
- MIT License


## Get Started

* Download and Install [Vagrant](https://www.vagrantup.com/downloads.html)
* Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* Clone this [GitHub Repository](https://github.com/rokde/laravel-box)
* Run ``` vagrant up ```
* Access Your Project at  [http://192.168.33.10/](http://192.168.33.10/)

## Basic Vagrant Commands


### Start or resume your server
```bash
vagrant up
```

### Pause your server
```bash
vagrant suspend
```

### Delete your server
```bash
vagrant destroy
```

### SSH into your server
```bash
vagrant ssh
```

## Database Access

### MySQL 

- Hostname: localhost or 127.0.0.1
- Username: root
- Password: root
- Database: scotchbox

### PostgreSQL

- Hostname: localhost or 127.0.0.1
- Username: root
- Password: root
- Database: scotchbox
- Port: 5432


### MongoDB

- Hostname: localhost
- Database: scotchbox
- Port: 27017


## SSH Access

- Hostname: 127.0.0.1:2222
- Username: vagrant
- Password: vagrant

## Mailcatcher

Just do:

```
vagrant ssh
mailcatcher --http-ip=0.0.0.0
```

Then visit:

```
http://192.168.33.10:1080
```


## Updating the Box

Although not necessary, if you want to check for updates, just type:

```bash
vagrant box outdated
```

It will tell you if you are running the latest version or not, of the box. If it says you aren't, simply run:

```bash
vagrant box update
```


## Setting a Hostname

If you're like me, you prefer to develop at a domain name versus an IP address. If you want to get rid of the some-what ugly IP address, just add a record like the following example to your computer's host file.

```bash
192.168.33.10 whatever-i-want.local
```

Or if you want "www" to work as well, do:

```bash
192.168.33.10 whatever-i-want.local www.whatever-i-want.local
```

Technically you could also use a Vagrant Plugin like [Vagrant Hostmanager](https://github.com/smdahlen/vagrant-hostmanager) to automatically update your host file when you run Vagrant Up. However, the purpose of Scotch Box is to have as little dependencies as possible so that it's always working when you run "vagrant up".
This plugin is fully supported from scratch.


## Configuration

You may want to change some of the out-of-the-box configurations for the various parts that come with Scotch Box. To do so, `vagrant ssh` into the box, and edit the appropriate file. For example, to change PHP settings:

    vagrant ssh
    sudo vim /etc/php5/apache2/conf.d/user.ini

Note that the changes that you make will be for the current running Scotch Box only. If you `vagrant destroy` and then `vagrant up` your box again, these manual configuration changes will be lost.

If you prefer to automate your configuration changes so that you can destroy and re-create boxes as needed, Vagrant allows you to create a "provision script" that runs as part of `vagrant up`.  See the [Vagrant documentation](https://docs.vagrantup.com/v2/getting-started/provisioning.html)
for notes. For example, you could add the following line to your Vagrantfile under the `config.vm.hostname = "scotchbox"` line:

    config.vm.provision :shell, path: "bootstrap.sh"

and then create `bootstrap.sh` with the following content in the same directory as the Vagrantfile:

    #!/bin/bash
    # Disable Zend OPcache
    sed -i 's/;opcache.enable=0/opcache.enable=0/g' /etc/php5/apache2/php.ini

This script will be run each time you `vagrant up`, and it can be run
on an already-up box using `vagrant provision`.
