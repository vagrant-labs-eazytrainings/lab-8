RAM = 1024
CPU = 1

Vagrant.configure("2") do |config|
    # Configure load balancer machine
     config.vm.define "lb" do |lb|
         lb.vm.box = "ubuntu/xenial64"
		 lb.vm.hostname = "lb"
         lb.vm.network "private_network", ip: "10.0.0.10"
		 lb.vm.provision :shell do |shell|
             shell.path = "https://raw.githubusercontent.com/PacktPublishing/Hands-On-DevOps-with-Vagrant/master/Chapter07/lb.sh"
         end
     end
	 config.vm.provider "virtualbox" do |v|
       v.memory = RAM
       v.cpus = CPU
  end
    # Configure first web machine
     config.vm.define "web1" do |web1|
         web1.vm.box = "ubuntu/xenial64"
		 web1.vm.hostname = "web1"
         web1.vm.network "private_network", ip: "10.0.0.11"
		 web1.vm.provision :shell do |shell|
             shell.path = "https://raw.githubusercontent.com/PacktPublishing/Hands-On-DevOps-with-Vagrant/master/Chapter07/web.sh"
		     shell.args = "1"
		 end
    end
	 config.vm.provider "virtualbox" do |v|
       v.memory = RAM
       v.cpus = CPU
    end
    # Configure second web machine
     config.vm.define "web2" do |web2|
         web2.vm.box = "ubuntu/xenial64"
		 web2.vm.hostname = "web2"
         web2.vm.network "private_network", ip: "10.0.0.12"
		 web2.vm.provision :shell do |shell|
             shell.path = "https://raw.githubusercontent.com/PacktPublishing/Hands-On-DevOps-with-Vagrant/master/Chapter07/web.sh"
             shell.args = "2"
		 end
     end
	config.vm.provider "virtualbox" do |v|
      v.memory = RAM
      v.cpus = CPU
    end
end
