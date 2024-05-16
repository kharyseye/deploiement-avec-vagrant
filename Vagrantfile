
Vagrant.configure("2") do |config|
  
  config.vm.define "server-app" do |devapp01|

    devapp01.vm.box = "bento/ubuntu-22.04"

    devapp01.vm.hostname = "devapp01"

    devapp01.vm.box_check_update = false

    devapp01.vm.network "forwarded_port", guest: 8080, host: 8082

    devapp01.vm.network "private_network", type: "static", ip: "172.16.238.10"

    devapp01.vm.synced_folder "./tomcatwebapps", "/opt/tomcat/webapps"

    devapp01.vm.synced_folder "./java", "/opt/java/"

    devapp01.vm.synced_folder "./git", "/opt/git/"

    devapp01.vm.provision "shell", inline: <<-SHELL
    
    sudo apt-get update
    #installation de git java et tomcat9 sur devapp01
    sudo apt-get install -y git
    sudo apt-get install -y default-jdk
    sudo apt-get install -y tomcat9
    #il faut installer postgresql client pour pouvoir utiliser la commande psql sur devapp01
    sudo apt-get install -y postgresql-client
  SHELL

    devapp01.vm.provider "virtualbox" do |vb|

      vb.gui = false
      vb.name = "devapp01"
      vb.memory = "1024"
    end
  end

  config.vm.define "server-db" do |dbapp01|

    dbapp01.vm.box = "bento/ubuntu-22.04"

    dbapp01.vm.hostname = "dbapp01"

    dbapp01.vm.box_check_update = false

    dbapp01.vm.network "forwarded_port", guest: 8080, host: 8083

    dbapp01.vm.network "private_network", type: "static", ip: "172.16.238.11"

    dbapp01.vm.synced_folder "./postgresql", "/opt/postgresql/"

    dbapp01.vm.provision "shell", inline: <<-SHELL
  
    sudo apt-get update
    #je commence par installer postgresql sur cette machine 
    sudo apt-get install -y postgresql postgresql-contrib
    #je change le mot de passe de mon superUtilisateur 
    sudo -u postgres psql -c "ALTER USER postgres PASSWORD 'passer';"
    #configuration du fichier postgresql.conf
    #dans listen addresses il faut ajouter l'adresse du serveur ou notre SGBD est installe
    sudo sed -i "s/#listen_addresses = 'localhost'/listen_addresses = '172.16.238.11'/" /etc/postgresql/14/main/postgresql.conf
    #configuration du fichier pg_hba.conf
    #sur ce fichier je dois ajouter ajouter le host le nom de ma base l'utilisateur l'@IP ainsi que la methode de connexion
    echo "host    userdb      postgres         172.16.238.10/24      md5" | sudo tee -a /etc/postgresql/14/main/pg_hba.conf
    #il faut imperativement redemarrer postgresql pour que les modifications soit prises en compte
    #Sisi cette etape est importante ;) donc faites le haha
    sudo systemctl restart postgresql
    #creation de ma base userdb j'aurais pu le faire avant aussi !
    sudo -u postgres psql -c "CREATE DATABASE userdb";
    SHELL

    dbapp01.vm.provider "virtualbox" do |vb|

      vb.gui = false
      vb.name = "dbapp01"
      vb.memory = "1024"
    end
  end

  config.vm.define "server-backup" do |backup01|

    backup01.vm.box = "bento/ubuntu-22.04"

    backup01.vm.hostname = "backup01"

    backup01.vm.box_check_update = false

    backup01.vm.network "forwarded_port", guest: 8080, host: 8084

    backup01.vm.network "private_network", type: "static", ip: "172.16.238.12"

    backup01.vm.provider "virtualbox" do |vb|

      vb.gui = false
      vb.name = "backup01"
      vb.memory = "1024"
    end
  end
end
