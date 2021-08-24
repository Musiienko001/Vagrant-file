BOX_IMAGE = "bento/ubuntu-20.04"
NODE_COUNT = 2

Vagrant.configure("2") do |config|
  config.vm.define "puppet.local" do |subconfig|
    subconfig.vm.box = BOX_IMAGE
    subconfig.vm.provision "shell", inline: <<-SHELL
    SHELL
    subconfig.vm.hostname = "puppet.local"
    subconfig.vm.network :private_network, ip: "192.168.56.5"
    config.vm.provider :virtualbox do |vb|
	 vb.customize ["modifyvm", :id, "--cableconnected0", "on"]
         vb.memory = "2000"
	 vb.gui = true
#         vb.cpus = 2
    end



subconfig.vm.provision "file", source: "./templates/id_rsa.pub", destination: "~/.ssh/authorized_keys"
    subconfig.vm.provision "file", source: "./templates/id_rsa", destination: "/home/vagrant/server_ca"
    subconfig.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
   SHELL



 subconfig.vm.provision "shell", inline: <<-SHELL
echo '192.168.56.5 puppetmaster.local puppet' >> /etc/hosts
echo '192.168.56.6 puppetclient.local' >> /etc/hosts
 wget https://apt.puppetlabs.com/puppet7-release-focal.deb 
sudo dpkg -i puppet7-release-focal.deb 
sudo apt -y update 
sudo apt install puppetserver -y
sudo apt-get -y install ntp 
sudo sed -i 's/.*JAVA_ARGS.*/JAVA_ARGS="-Xms512m -Xmx512m"/' /etc/default/puppetserver
echo '[agent]' >> /etc/puppetlabs/puppet/puppet.conf
echo runinterval=300 >> /etc/puppetlabs/puppet/puppet.conf
sudo hostname puppetmaster.local
sudo systemctl start puppetserver
sudo systemctl enable puppetserver
sudo service puppet start
sudo /opt/puppetlabs/bin/puppetserver ca list --all 
 
SHELL

end



  config.vm.define "jenkins.local" do |subconfig|
    subconfig.vm.box = BOX_IMAGE
    subconfig.vm.hostname = "jenkins.local"
    subconfig.vm.network :private_network, ip: "192.168.56.8"
    config.vm.provider :virtualbox do |vb|
	 vb.customize ["modifyvm", :id, "--cableconnected0", "on"]
         vb.memory = "2000"
	 vb.gui = true
#         vb.cpus = 2
    end
    
   
     
    subconfig.vm.provision "file", source: "./templates/id_rsa.pub", destination: "~/.ssh/authorized_keys"
    subconfig.vm.provision "file", source: "./templates/id_rsa", destination: "/home/vagrant/server_ca"
    subconfig.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
   SHELL
  end

  config.vm.define "nginx.local" do |subconfig|
    subconfig.vm.box = BOX_IMAGE
    subconfig.vm.hostname = "nginx.local"
    subconfig.vm.network :private_network, ip: "192.168.56.6"
    config.vm.provider :virtualbox do |vb|
	 vb.customize ["modifyvm", :id, "--cableconnected0", "on"]
         vb.memory = "2000"
	 vb.gui = true
#         vb.cpus = 2
    end
    subconfig.vm.provision "file", source: "./templates/id_rsa.pub", destination: "~/.ssh/authorized_keys"
    subconfig.vm.provision "file", source: "./templates/id_rsa", destination: "/home/vagrant/server_ca"
    subconfig.vm.provision "shell", inline: <<-SHELL
   SHELL


subconfig.vm.provision "shell", inline: <<-SHELL
echo '192.168.56.5 puppetmaster.local puppet' >> /etc/hosts
echo '192.168.56.6 puppetclient.local' >> /etc/hosts
wget https://apt.puppetlabs.com/puppet7-release-focal.deb
sudo dpkg -i puppet7-release-focal.deb
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 4528B6CD9E61EF26
sudo apt update -y
sudo apt install puppet-agent -y
sudo apt-get -y install ntp
sudo sed -i 's/.*JAVA_ARGS.*/JAVA_ARGS="-Xms512m -Xmx512m"/' /etc/default/puppetserver
echo '[main]' >> /etc/puppetlabs/puppet/puppet.conf
echo 'certname = 'puppetclient.local'' >> /etc/puppetlabs/puppet/puppet.conf
echo 'certname = 'puppetmaster.local puppet'' >> /etc/puppetlabs/puppet/puppet.conf
cd /opt/puppetlabs/bin
./puppet agent --server puppetmaster.local --waitforcert 60 --test
sudo systemctl start puppet
sudo systemctl enable puppet
sudo /opt/puppetlabs/bin/puppet agent --test 
echo runinterval=200 >> /etc/puppetlabs/puppet/puppet.conf
SHELL
  end



  (1..NODE_COUNT).each do |i|
    config.vm.define "node#{i}.local" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.hostname = "node#{i}.local"
      subconfig.vm.network :private_network, ip: "192.168.56.#{i + 50}"
      config.vm.provider :virtualbox do |vb|
       	 vb.customize ["modifyvm", :id, "--cableconnected0", "on"]
         vb.memory = "2000"
	 vb.gui = true
#         vb.cpus = 2
      end
      subconfig.vm.provision "file", source: "./templates/id_rsa.pub", destination: "~/.ssh/authorized_keys"
      subconfig.vm.provision "file", source: "./templates/id_rsa", destination: "/home/vagrant/server_ca"
      subconfig.vm.provision "shell", inline: <<-SHELL
      SHELL
end    
  end
 end
