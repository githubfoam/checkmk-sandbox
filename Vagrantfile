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
    hostnamectl set-hostname vg-checkmk-srv
    echo "172.28.128.12 vg-checkmk-srv.local vg-checkmk-srv" |tee -a /etc/hosts
    echo "name: nameserver, ip: 8.8.8.8 " |tee -a /etc/resolv.conf
		apt-get update && apt-get -y install gdebi-core
    wget -q https://checkmk.com/support/Check_MK-pubkey.gpg
    wget -q https://checkmk.com/support/1.6.0p8/check-mk-raw-1.6.0p8_0.xenial_amd64.deb
		apt-key add Check_MK-pubkey.gpg
    gdebi -n check-mk-raw-1.6.0p8_0.xenial_amd64.deb
		omd create monitor
		omd start monitor
    wget -q http://172.28.128.12/monitor/check_mk/agents/check-mk-agent_1.6.0p8_1_all.deb
		gdebi -n check-mk-agent_1.6.0p8_1_all.debb
		cd /omd/sites/monitor/etc
		rm htpasswd
		# su
		htpasswd -nb cmkadmin admin > htpasswd
		# exit
    omd version
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
  hostnamectl set-hostname vg-checkmk-client
  echo "172.28.128.15 vg-checkmk-client.local vg-checkmk-client" |tee -a /etc/hosts
  echo "name: nameserver, ip: 8.8.8.8 " |tee -a /etc/resolv.conf
  apt-get update && apt-get -y install gdebi-core xinetd
  wget -q http://172.28.128.12/monitor/check_mk/agents/check-mk-agent_1.6.0p8-1_all.deb
	gdebi -n check-mk-agent_1.6.0p8-1_all.deb
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
