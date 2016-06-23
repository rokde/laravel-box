# -*- mode: ruby -*-
# vi: set ft=ruby :

module OS
    def OS.windows?
        (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    end

    def OS.mac?
        (/darwin/ =~ RUBY_PLATFORM) != nil
    end

    def OS.unix?
        !OS.windows?
    end

    def OS.linux?
        OS.unix? and not OS.mac?
    end
end

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "scotchbox"

    config.ssh.username = "vagrant"
    config.ssh.password = "vagrant"

    if Vagrant.has_plugin?("vagrant-hostsupdater") then
        config.hostsupdater.remove_on_suspend = false
    end

    # if a VPN connection is active on the host system, direct routing might fail,
    # so we redirect, ie. 127.0.0.1:8090 => 192.168.33.10:80
    config.vm.network "forwarded_port", guest: 80, host: 8090, protocol: "tcp"

    if OS.windows? then
        config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=777"], :create => true, :owner => 'vagrant', :group => 'vagrant' }
    else
        config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
    end

    config.vm.provider :virtualbox do |vb|
        #vb.name = "scotchbox"
        vb.customize ["modifyvm", :id, "--memory", "2048"]
        vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
        vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/var/www", "1"]
    end

    $script = <<-SCRIPT
        # update to php-7
        sudo apt-get update -y -qq
        sudo add-apt-repository ppa:ondrej/php -y
        sudo apt-get install php7.0 -y -qq
        sudo apt-get update -y -qq
        sudo apt-get install php7.0-mysql libapache2-mod-php7.0 -y -qq
        sudo a2dismod php5
        sudo a2enmod php7.0
        sudo apache2ctl restart

        # install dependencies for laravel-elixir
        sudo apt-get install libnotify-bin -y -qq

        # set start directory after ssh-login
        echo "cd /var/www" >> /home/vagrant/.profile
        echo "cd /var/www" >> /home/vagrant/.bashrc
        
        # update built-in composer
        sudo /usr/local/bin/composer self-update -q
        
        # install laravel dependencies
        composer install -q
        
        # install npm
        npm install --save --silent

        # set locale to de_DE
        sudo locale-gen de_DE.UTF-8
    SCRIPT

    config.vm.provision "shell", inline: $script, privileged: false

end
