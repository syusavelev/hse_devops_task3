# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  config.vm.define "apache_host" do |apache_host|
  	  apache_host.vm.network "private_network", ip: "127.0.0.1"
  	  apache_host.vm.network "forwarded_port", guest: 80, host: 80
  	  apache_host.vm.provision "shell", inline: <<-SHELL
  	    apt-get install -y apache2
        apt install libapache2-mod-wsgi
        apache2ctl -t -D DUMP_MODULES
        mkdir -p /var/www/hello_world/public_html
      SHELL

      apache_host.vm.provision "file", source: "index.html", destination: "/tmp/index.html"
      apache_host.vm.provision "file", source: "hello.world.conf", destination: "/tmp/hello.world.conf"

      apache_host.vm.provision "shell", inline: <<-SHELL
        mv /tmp/index.html /var/www/hello_world/public_html/
        mv /tmp/hello.world.conf /etc/apache2/sites-available/
      SHELL

      apache_host.vm.provision "shell", inline: <<-SHELL
        mkdir -p /var/www/hello_py/public_html
      SHELL

      apache_host.vm.provision "file", source: "hello.wsgi", destination: "/tmp/hello.wsgi"
      apache_host.vm.provision "file", source: "hello.py.conf", destination: "/tmp/hello.py.conf"

      apache_host.vm.provision "shell", inline: <<-SHELL
        mv /tmp/hello.wsgi /var/www/hello_py/public_html/
        mv /tmp/hello.py.conf /etc/apache2/sites-available/
      SHELL

      apache_host.vm.provision "file", source: "hosts", destination: "/tmp/hosts"

      apache_host.vm.provision "shell", inline: <<-SHELL
        cat /tmp/hosts >> /etc/hosts
  	  SHELL

      apache_host.vm.provision "shell", path: "en.sh"
    end
end
