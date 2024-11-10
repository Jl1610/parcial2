# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    # Configuración para el servidor web1
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/jammy64"
      web1.vm.hostname = "web1"
      web1.vm.network "private_network", ip: "192.168.56.10"
      web1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web1.vm.synced_folder "./web1", "/var/www/html", create: true
      web1.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
      SHELL
    end
  
    # Configuración para el servidor web2
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/jammy64"
      web2.vm.hostname = "web2"
      web2.vm.network "private_network", ip: "192.168.56.11"
      web2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web2.vm.synced_folder "./web2", "/var/www/html", create: true
      web2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
      SHELL
    end
  
    # Configuración para el servidor loadbalancer
    config.vm.define "loadbalancer" do |lb|
      lb.vm.box = "ubuntu/jammy64"
      lb.vm.hostname = "loadbalancer"
      lb.vm.network "private_network", ip: "192.168.56.12"
      lb.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      lb.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y nginx
        sudo tee /etc/nginx/conf.d/load_balancer.conf > /dev/null <<EOL
  upstream backend {
      server 192.168.56.10;
      server 192.168.56.11;
  }
  
  server {
      listen 80;
  
      location / {
          proxy_pass http://backend;
      }
  }
  EOL
        sudo systemctl restart nginx
      SHELL
    end
  
  end
  