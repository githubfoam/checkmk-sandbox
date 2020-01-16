#
#
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "vg-checkmk-srv" do |checkmk|
    checkmk.vm.box = "bento/ubuntu-16.04"
    checkmk.vm.hostname = "vbox-checkmk-srv"
    checkmk.vm.network "private_network", ip:"172.28.128.12"
		checkmk.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
		checkmk.vm.provider "virtualbox" do |vb|
	  vb.memory = "1024"
	end
	checkmk.vm.provision "shell", inline: <<-SHELL
		sudo apt-get update
    sudo wget -q https://checkmk.com/support/Check_MK-pubkey.gpg
    sudo wget -q https://checkmk.com/support/1.6.0p8/check-mk-raw-1.6.0p8_0.xenial_amd64.deb
		sudo apt-key add Check_MK-pubkey.gpg
		sudo apt-get -y install gdebi-core
    sudo gdebi -n check-mk-raw-1.6.0p8_0.xenial_amd64.deb
		sudo omd create monitor
		sudo omd start monitor
    wget -q http://172.28.128.12/monitor/check_mk/agents/check-mk-agent_1.6.0p8_1_all.deb
    wget -q http://172.28.128.12/monitor/check_mk/agents/check-mk-agent_1.6.0p8_1_all.deb
		sudo gdebi -n check-mk-agent_1.6.0p8_1_all.deb
                  check-mk-agent_1.6.0p8-1_all.deb
		cd /omd/sites/monitor/etc
		sudo rm htpasswd
		sudo su
		htpasswd -nb cmkadmin admin > htpasswd
		exit
    echo "===================================================================================="
                              hostnamectl status
    echo "===================================================================================="
    echo "         \   ^__^                                                                  "
    echo "          \  (oo)\_______                                                          "
    echo "             (__)\       )\/\                                                      "
    echo "                 ||----w |                                                         "
    echo "                 ||     ||                                                         "
	SHELL
	end

	config.vm.define "vg-checkmk-client" do |client|
    client.vm.box = "bento/ubuntu-16.04"
    client.vm.hostname = "vbox-checkmk-client"
    client.vm.network "private_network", ip:"172.28.128.15"
	client.vm.provider "virtualbox" do |vb|
	  vb.memory = "1024"
	end
	client.vm.provision "shell", inline: <<-SHELL
	sudo apt-get -y install gdebi-core
	sudo apt-get -y install xinetd
  wget -q http://172.28.128.12/monitor/check_mk/agents/check-mk-agent_1.6.0p8-1_all.deb
	sudo gdebi -n check-mk-agent_1.6.0p8-1_all.deb
  echo "===================================================================================="
                            hostnamectl status
  echo "===================================================================================="
  echo "         \   ^__^                                                                  "
  echo "          \  (oo)\_______                                                          "
  echo "             (__)\       )\/\                                                      "
  echo "                 ||----w |                                                         "
  echo "                 ||     ||                                                         "
SHELL
	end

 end
