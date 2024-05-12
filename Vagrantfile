
Vagrant.configure("2") do |config|
  
  config.vm.define "server-app" do |devapp01|

    devapp01.vm.box = "bento/ubuntu-22.04"

    devapp01.vm.box_check_update = false

    devapp01.vm.network "forwarded_port", guest: 8080, host: 8082

    devapp01.vm.network "private_network", type: "static", ip: "172.16.238.10"

    devapp01.vm.synced_folder "./tomcatwebapps", "/opt/tomcat/webapps"

    devapp01.vm.synced_folder "./java", "/opt/java/"

    devapp01.vm.synced_folder "./git", "/opt/git/"

    devapp01.vm.provision "shell", inline: <<-SHELL
    
    sudo apt-get update
    sudo apt-get install -y git
    sudo apt-get install -y default-jdk
    sudo apt-get install -y tomcat9
  SHELL

    devapp01.vm.provider "virtualbox" do |vb|

      vb.gui = false
      vb.name = "devapp01"
      vb.memory = "1024"
    end
  end

  config.vm.define "server-db" do |dbapp01|

    dbapp01.vm.box = "bento/ubuntu-22.04"

    dbapp01.vm.box_check_update = false

    dbapp01.vm.network "forwarded_port", guest: 8080, host: 8083

    dbapp01.vm.network "private_network", type: "static", ip: "172.16.238.11"

    dbapp01.vm.synced_folder "./postgresql", "/opt/postgresql/"

    dbapp01.vm.provision "shell", inline: <<-SHELL
    # Mise à jour des paquets
    sudo apt-get update
    # Installation de PostgreSQL
    sudo apt-get install -y postgresql postgresql-contrib
    SHELL

    dbapp01.vm.provider "virtualbox" do |vb|

      vb.gui = false
      vb.name = "dbapp01"
      vb.memory = "1024"
    end
  end

  config.vm.define "server-backup" do |backup01|

    backup01.vm.box = "bento/ubuntu-22.04"

    backup01.vm.box_check_update = false

    backup01.vm.network "forwarded_port", guest: 8080, host: 8084

    backup01.vm.network "private_network", type: "static", ip: "172.16.238.12"

    #dbapp01.vm.synced_folder "./postgresql", "/opt/postgresql/"

    backup01.vm.provider "virtualbox" do |vb|

      vb.gui = false
      vb.name = "backup01"
      vb.memory = "1024"
    end
  end
end
