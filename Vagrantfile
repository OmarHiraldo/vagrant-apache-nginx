Vagrant.configure("2") do |config|

    # Configuración común para todas las máquinas
    config.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
  
    # Servidor Web Apache 1
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/bionic64"
      web1.vm.hostname = "web1"
      web1.vm.network "private_network", type: "static", ip: "192.168.33.10", virtualbox__intnet: true
  
      # Directorio compartido
      web1.vm.synced_folder ".", "/var/www/html", type: "virtualbox", SharedFoldersEnableSymlinksCreate: false
  
      # Instalación de Apache
      web1.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
      SHELL
    end
  
    # Servidor Web Apache 2
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/bionic64"
      web2.vm.hostname = "web2"
      web2.vm.network "private_network", type: "static", ip: "192.168.33.11", virtualbox__intnet: true
  
      # Directorio compartido
      web2.vm.synced_folder ".", "/var/www/html", type: "virtualbox", SharedFoldersEnableSymlinksCreate: false
  
      # Instalación de Apache
      web2.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y apache2
        sudo systemctl enable apache2
        sudo systemctl start apache2
      SHELL
    end
  
    # Servidor Nginx (Balanceador de carga)
    config.vm.define "nginx" do |nginx|
      nginx.vm.box = "ubuntu/bionic64"
      nginx.vm.hostname = "nginx"
      nginx.vm.network "private_network", type: "static", ip: "192.168.33.20", virtualbox__intnet: true
  
      # Instalación de Nginx
      nginx.vm.provision "shell", inline: <<-SHELL
        sudo apt-get update
        sudo apt-get install -y nginx
        sudo systemctl enable nginx
        sudo systemctl start nginx
      SHELL
  
      # Copiar nginx.conf desde la máquina host a la máquina virtual y moverlo a la ubicación correcta
      nginx.vm.provision "file", source: "nginx.conf", destination: "/tmp/nginx.conf"
  
      # Script para mover nginx.conf a la ubicación correcta y asignar permisos
      nginx.vm.provision "shell", inline: <<-SHELL
        sudo mv /tmp/nginx.conf /etc/nginx/nginx.conf
        sudo chown root:root /etc/nginx/nginx.conf
        sudo chmod 644 /etc/nginx/nginx.conf
        sudo systemctl restart nginx
      SHELL
    end
  
  end
  