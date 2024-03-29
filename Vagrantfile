# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  #configuracao da instancia do node1
  config.vm.define "node1" do |hadoop|
    hadoop.vm.box = "ubuntu/xenial64"

    hadoop.vm.network "private_network", ip: "192.168.56.41"
    hadoop.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)", ip: "192.168.15.41"
    hadoop.vm.hostname = "node1.infobarbosa.github.com"
    hadoop.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 1
      v.name = "node1-lab-hadoop.vagrant"
    end

    hadoop.vm.provision "file", source: "config-files/core-site.xml", destination: "core-site.xml"
    hadoop.vm.provision "file", source: "config-files/hdfs-site.xml", destination: "hdfs-site.xml"
    hadoop.vm.provision "file", source: "config-files/yarn-site.xml", destination: "yarn-site.xml"
    hadoop.vm.provision "file", source: "config-files/mapred-site.xml", destination: "mapred-site.xml"
    hadoop.vm.provision "file", source: "config-files/hosts", destination: "hosts"
    hadoop.vm.provision "file", source: "config-files/master_key.pub", destination: "master_key.pub"
    hadoop.vm.provision "file", source: "packages/hadoop-3.2.0.tar.gz", destination: "hadoop-3.2.0.tar.gz"

    #root script
    $script1 = <<-SCRIPT

      #hosts
      cat hosts >> /etc/hosts
      sed -i "s/127.0.1.1/#127.0.1.1/" /etc/hosts

      useradd -m hadoop
      echo "hadoop:hadoop" | chpasswd

      locale-gen en_US.UTF-8
      add-apt-repository ppa:openjdk-r/ppa
      apt-get -y update && apt-get install -y openjdk-8-jdk net-tools

      echo "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre" >> /etc/profile
      source /etc/profile
      echo "export PATH=${JAVA_HOME}/bin:${PATH}" >> /etc/profile
      source /etc/profile

      #wget -qO- http://ftp.unicamp.br/pub/apache/hadoop/common/hadoop-3.2.0/hadoop-3.2.0.tar.gz | tar xvz -C /opt/
      tar xzf hadoop-3.2.0.tar.gz
      mv hadoop-3.2.0 /opt/hadoop
      chown -R hadoop:hadoop /opt/hadoop
  
      #variaveis de ambiente
      echo "export HADOOP_HOME=/opt/hadoop" >> /etc/profile
      source /etc/profile
      echo "export PATH=${PATH}:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin" >> /etc/profile
      source /etc/profile

      sed -i "/#[[:space:]]*export[[:space:]]*JAVA_HOME=/ s/#[[:space:]]*export[[:space:]]*JAVA_HOME=.*/export JAVA_HOME=/" /opt/hadoop/etc/hadoop/hadoop-env.sh
      sed -i "/export[[:space:]]JAVA_HOME=/ s:=.*:=/usr/lib/jvm/java-8-openjdk-amd64/jre:" /opt/hadoop/etc/hadoop/hadoop-env.sh
      
      su hadoop -c 'touch /opt/hadoop/etc/hadoop/workers' 
      echo "node1" >> /opt/hadoop/etc/hadoop/workers
      echo "node2" >> /opt/hadoop/etc/hadoop/workers
      echo "node3" >> /opt/hadoop/etc/hadoop/workers

      su hadoop -c 'mkdir -p /home/hadoop/.ssh'
      chmod 700 /home/hadoop/.ssh
      mv /home/vagrant/master_key.pub /home/hadoop/.ssh
      chown -R hadoop:hadoop /home/hadoop/.ssh/*
      su hadoop -c 'touch /home/hadoop/.ssh/authorized_keys'
      su hadoop -c 'cat /home/hadoop/.ssh/master_key.pub > /home/hadoop/.ssh/authorized_keys'
      chmod 600 /home/hadoop/.ssh/*

      mv /home/vagrant/*.xml /opt/hadoop/etc/hadoop/
      chown -R hadoop:hadoop /opt/hadoop/etc/hadoop/      
    SCRIPT

    hadoop.vm.provision "shell", inline: $script1
  end

  ###########################
  #configuracao da instancia do node2
  config.vm.define "node2" do |hadoop|
    hadoop.vm.box = "ubuntu/xenial64"

    hadoop.vm.network "private_network", ip: "192.168.56.42"
    hadoop.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)", ip: "192.168.15.42"
    hadoop.vm.hostname = "node2.infobarbosa.github.com"
    hadoop.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 1
      v.name = "node2-lab-hadoop.vagrant"
    end

    hadoop.vm.provision "file", source: "config-files/core-site.xml", destination: "core-site.xml"
    hadoop.vm.provision "file", source: "config-files/hdfs-site.xml", destination: "hdfs-site.xml"
    hadoop.vm.provision "file", source: "config-files/yarn-site.xml", destination: "yarn-site.xml"
    hadoop.vm.provision "file", source: "config-files/mapred-site.xml", destination: "mapred-site.xml"
    hadoop.vm.provision "file", source: "config-files/hosts", destination: "hosts"
    hadoop.vm.provision "file", source: "config-files/master_key.pub", destination: "master_key.pub"
    hadoop.vm.provision "file", source: "packages/hadoop-3.2.0.tar.gz", destination: "hadoop-3.2.0.tar.gz"

    #root script
    $script1 = <<-SCRIPT

      #hosts
      cat hosts >> /etc/hosts
      sed -i "s/127.0.1.1/#127.0.1.1/" /etc/hosts

      useradd -m hadoop
      echo "hadoop:hadoop" | chpasswd

      locale-gen en_US.UTF-8
      add-apt-repository ppa:openjdk-r/ppa
      apt-get -y update && apt-get install -y openjdk-8-jdk net-tools

      echo "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre" >> /etc/profile
      source /etc/profile
      echo "export PATH=${JAVA_HOME}/bin:${PATH}" >> /etc/profile
      source /etc/profile

      #wget -qO- http://ftp.unicamp.br/pub/apache/hadoop/common/hadoop-3.2.0/hadoop-3.2.0.tar.gz | tar xvz -C /opt/
      tar xzf hadoop-3.2.0.tar.gz
      mv hadoop-3.2.0 /opt/hadoop
      chown -R hadoop:hadoop /opt/hadoop
  
      #variaveis de ambiente
      echo "export HADOOP_HOME=/opt/hadoop" >> /etc/profile
      source /etc/profile
      echo "export PATH=${PATH}:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin" >> /etc/profile
      source /etc/profile

      sed -i "/#[[:space:]]*export[[:space:]]*JAVA_HOME=/ s/#[[:space:]]*export[[:space:]]*JAVA_HOME=.*/export JAVA_HOME=/" /opt/hadoop/etc/hadoop/hadoop-env.sh
      sed -i "/export[[:space:]]JAVA_HOME=/ s:=.*:=/usr/lib/jvm/java-8-openjdk-amd64/jre:" /opt/hadoop/etc/hadoop/hadoop-env.sh
      
      su hadoop -c 'touch /opt/hadoop/etc/hadoop/workers' 
      echo "node1" >> /opt/hadoop/etc/hadoop/workers
      echo "node2" >> /opt/hadoop/etc/hadoop/workers
      echo "node3" >> /opt/hadoop/etc/hadoop/workers

      su hadoop -c 'mkdir -p /home/hadoop/.ssh'
      chmod 700 /home/hadoop/.ssh
      mv /home/vagrant/master_key.pub /home/hadoop/.ssh
      chown -R hadoop:hadoop /home/hadoop/.ssh/*
      su hadoop -c 'touch /home/hadoop/.ssh/authorized_keys'
      su hadoop -c 'cat /home/hadoop/.ssh/master_key.pub > /home/hadoop/.ssh/authorized_keys'
      chmod 600 /home/hadoop/.ssh/*

      mv /home/vagrant/*.xml /opt/hadoop/etc/hadoop/
      chown -R hadoop:hadoop /opt/hadoop/etc/hadoop/

    SCRIPT

    hadoop.vm.provision "shell", inline: $script1
  end

  #####################
  #configuracao da instancia do node3
  config.vm.define "node3" do |hadoop|
    hadoop.vm.box = "ubuntu/xenial64"

    hadoop.vm.network "private_network", ip: "192.168.56.43"
    hadoop.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)", ip: "192.168.15.43"
    hadoop.vm.hostname = "node3.infobarbosa.github.com"
    hadoop.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 1
      v.name = "node3-lab-hadoop.vagrant"
    end

    hadoop.vm.provision "file", source: "config-files/core-site.xml", destination: "core-site.xml"
    hadoop.vm.provision "file", source: "config-files/hdfs-site.xml", destination: "hdfs-site.xml"
    hadoop.vm.provision "file", source: "config-files/yarn-site.xml", destination: "yarn-site.xml"
    hadoop.vm.provision "file", source: "config-files/mapred-site.xml", destination: "mapred-site.xml"
    hadoop.vm.provision "file", source: "config-files/hosts", destination: "hosts"
    hadoop.vm.provision "file", source: "config-files/master_key.pub", destination: "master_key.pub"
    hadoop.vm.provision "file", source: "packages/hadoop-3.2.0.tar.gz", destination: "hadoop-3.2.0.tar.gz"

    #root script
    $script1 = <<-SCRIPT

      #hosts
      cat hosts >> /etc/hosts
      sed -i "s/127.0.1.1/#127.0.1.1/" /etc/hosts

      useradd -m hadoop
      echo "hadoop:hadoop" | chpasswd

      locale-gen en_US.UTF-8
      add-apt-repository ppa:openjdk-r/ppa
      apt-get -y update && apt-get install -y openjdk-8-jdk net-tools

      echo "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre" >> /etc/profile
      source /etc/profile
      echo "export PATH=${JAVA_HOME}/bin:${PATH}" >> /etc/profile
      source /etc/profile

      #wget -qO- http://ftp.unicamp.br/pub/apache/hadoop/common/hadoop-3.2.0/hadoop-3.2.0.tar.gz | tar xvz -C /opt/
      tar xzf hadoop-3.2.0.tar.gz
      mv hadoop-3.2.0 /opt/hadoop
      chown -R hadoop:hadoop /opt/hadoop
  
      #variaveis de ambiente
      echo "export HADOOP_HOME=/opt/hadoop" >> /etc/profile
      source /etc/profile
      echo "export PATH=${PATH}:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin" >> /etc/profile
      source /etc/profile

      sed -i "/#[[:space:]]*export[[:space:]]*JAVA_HOME=/ s/#[[:space:]]*export[[:space:]]*JAVA_HOME=.*/export JAVA_HOME=/" /opt/hadoop/etc/hadoop/hadoop-env.sh
      sed -i "/export[[:space:]]JAVA_HOME=/ s:=.*:=/usr/lib/jvm/java-8-openjdk-amd64/jre:" /opt/hadoop/etc/hadoop/hadoop-env.sh
      
      su hadoop -c 'touch /opt/hadoop/etc/hadoop/workers' 
      echo "node1" >> /opt/hadoop/etc/hadoop/workers
      echo "node2" >> /opt/hadoop/etc/hadoop/workers
      echo "node3" >> /opt/hadoop/etc/hadoop/workers

      su hadoop -c 'mkdir -p /home/hadoop/.ssh'
      chmod 700 /home/hadoop/.ssh
      mv /home/vagrant/master_key.pub /home/hadoop/.ssh
      chown -R hadoop:hadoop /home/hadoop/.ssh/*
      su hadoop -c 'touch /home/hadoop/.ssh/authorized_keys'
      su hadoop -c 'cat /home/hadoop/.ssh/master_key.pub > /home/hadoop/.ssh/authorized_keys'
      chmod 600 /home/hadoop/.ssh/*

      mv /home/vagrant/*.xml /opt/hadoop/etc/hadoop/
      chown -R hadoop:hadoop /opt/hadoop/etc/hadoop/
    SCRIPT

    hadoop.vm.provision "shell", inline: $script1
  end

  #####################
  #configuracao da instancia do node master
  config.vm.define "master" do |hadoop|
    hadoop.vm.box = "ubuntu/xenial64"

    hadoop.vm.network "private_network", ip: "192.168.56.40"
    hadoop.vm.network "public_network", bridge: "en0: Wi-Fi (AirPort)", ip: "192.168.15.40"
    hadoop.vm.hostname = "master.infobarbosa.github.com"
    hadoop.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 1
      v.name = "master-lab-hadoop.vagrant"
    end

    hadoop.vm.provision "file", source: "config-files/hadoop.service", destination: "hadoop.service"
    hadoop.vm.provision "file", source: "config-files/core-site.xml", destination: "core-site.xml"
    hadoop.vm.provision "file", source: "config-files/hdfs-site.xml", destination: "hdfs-site.xml"
    hadoop.vm.provision "file", source: "config-files/yarn-site.xml", destination: "yarn-site.xml"
    hadoop.vm.provision "file", source: "config-files/mapred-site.xml", destination: "mapred-site.xml"
    hadoop.vm.provision "file", source: "config-files/hosts", destination: "hosts"
    hadoop.vm.provision "file", source: "config-files/master_key", destination: "master_key"
    hadoop.vm.provision "file", source: "config-files/master_key.pub", destination: "master_key.pub"
    hadoop.vm.provision "file", source: "packages/hadoop-3.2.0.tar.gz", destination: "hadoop-3.2.0.tar.gz"

    #root script
    $script1 = <<-SCRIPT

      #hosts
      cat hosts >> /etc/hosts
      sed -i "s/127.0.1.1/#127.0.1.1/" /etc/hosts

      useradd -m hadoop
      echo "hadoop:hadoop" | chpasswd

      locale-gen en_US.UTF-8
      add-apt-repository ppa:openjdk-r/ppa
      apt-get -y update && apt-get install -y openjdk-8-jdk net-tools

      echo "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre" >> /etc/profile
      source /etc/profile
      echo "export PATH=${JAVA_HOME}/bin:${PATH}" >> /etc/profile
      source /etc/profile

      #wget -qO- http://ftp.unicamp.br/pub/apache/hadoop/common/hadoop-3.2.0/hadoop-3.2.0.tar.gz | tar xvz -C /opt/
      tar xzf hadoop-3.2.0.tar.gz
      mv hadoop-3.2.0 /opt/hadoop
      chown -R hadoop:hadoop /opt/hadoop
  
      #variaveis de ambiente
      echo "export HADOOP_HOME=/opt/hadoop" >> /etc/profile
      source /etc/profile
      echo "export PATH=${PATH}:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin" >> /etc/profile
      source /etc/profile

      sed -i "/#[[:space:]]*export[[:space:]]*JAVA_HOME=/ s/#[[:space:]]*export[[:space:]]*JAVA_HOME=.*/export JAVA_HOME=/" /opt/hadoop/etc/hadoop/hadoop-env.sh
      sed -i "/export[[:space:]]JAVA_HOME=/ s:=.*:=/usr/lib/jvm/java-8-openjdk-amd64/jre:" /opt/hadoop/etc/hadoop/hadoop-env.sh
      
      su hadoop -c 'touch /opt/hadoop/etc/hadoop/workers' 
      echo "node1" >> /opt/hadoop/etc/hadoop/workers
      echo "node2" >> /opt/hadoop/etc/hadoop/workers
      echo "node3" >> /opt/hadoop/etc/hadoop/workers

      su hadoop -c 'mkdir -p /home/hadoop/.ssh'
      chmod 700 /home/hadoop/.ssh
      mv /home/vagrant/master_key /home/hadoop/.ssh/id_rsa
      mv /home/vagrant/master_key.pub /home/hadoop/.ssh/id_rsa.pub
      chown -R hadoop:hadoop /home/hadoop/.ssh/*
      su hadoop -c 'touch /home/hadoop/.ssh/authorized_keys'
      su hadoop -c 'cat /home/hadoop/.ssh/id_rsa.pub > /home/hadoop/.ssh/authorized_keys'
      chmod 600 /home/hadoop/.ssh/*

      mv /home/vagrant/*.xml /opt/hadoop/etc/hadoop/
      chown -R hadoop:hadoop /opt/hadoop/etc/hadoop/

      #format hdfs
      su hadoop -c "/opt/hadoop/bin/hdfs namenode -format"

      #hdfs service
      mv hadoop.service /etc/systemd/system/
      chown hadoop:hadoop /etc/systemd/system/hadoop.service
      systemctl enable hadoop.service
      systemctl daemon-reload
      systemctl start hadoop

    SCRIPT

    hadoop.vm.provision "shell", inline: $script1
  end

end
