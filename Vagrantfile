#
#

$ubuntu_docker_script = <<SCRIPT
# Get Docker Engine - Community for Ubuntu
# https://docs.docker.com/install/linux/docker-ce/ubuntu/
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update
sudo apt-get install \
apt-transport-https \
ca-certificates \
curl \
software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) \
stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
sudo docker --version
# Manage Docker as a non-root user
# https://docs.docker.com/install/linux/linux-postinstall/
sudo groupadd docker && sudo usermod -aG docker vagrant # add user to the docker group
sudo systemctl enable docker
docker --version
# Install Terraform
sudo apt-get install unzip -y
wget -q -nc https://releases.hashicorp.com/terraform/0.12.18/terraform_0.12.18_linux_amd64.zip
unzip terraform_0.12.18_linux_amd64.zip
sudo mv terraform /usr/local/bin/
terraform version
SCRIPT
# https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
$ubuntu_docker_build_script = <<SCRIPT
whoami
sudo docker --version
sudo docker build -t holamundo:v4 -<<EOF
FROM ubuntu:19.04

# Set multiple labels at once, using line-continuation characters to break long lines
LABEL vendor="ACME Incorporated" \
      com.example.is-beta= \
      com.example.is-production="" \
      com.example.version="0.0.1-beta" \
      com.example.release-date="2015-02-12"

# Always combine RUN apt-get update with apt-get install in the same RUN statement
# Using apt-get update alone in a RUN statement causes caching issues and subsequent apt-get install instructions fail.
RUN apt-get update && apt-get install -y curl \
    nginx \
    git \
 && rm -rf /var/lib/apt/lists/*

RUN echo "hola mundo-4"
EOF
echo "===================================================================================="
SCRIPT
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "vg-checkmk-srv" do |checkmk|
    checkmk.vm.box = "bento/ubuntu-19.04"
    checkmk.vm.hostname = "vbox-checkmk-srv"
    checkmk.vm.network "private_network", ip:"172.28.128.12"
		checkmk.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
		checkmk.vm.provider "virtualbox" do |vb|
	  vb.memory = "1024"
	end
	checkmk.vm.provision "shell", inline: <<-SHELL
    hostnamectl set-hostname vg-checkmk-srv
    echo "172.28.128.12 vg-checkmk-srv.local vg-checkmk-srv" | tee -a /etc/hosts
    echo "172.28.128.15 vg-client-01.local vg-client-01" | tee -a /etc/hosts
    echo "172.28.128.16 vg-client-02.local vg-client-02" | tee -a /etc/hosts
    echo "name: nameserver, ip: 8.8.8.8 " |tee -a /etc/resolv.conf
		apt-get update && apt-get -y install gdebi-core
    wget -q https://checkmk.com/support/Check_MK-pubkey.gpg
    wget -q https://checkmk.com/support/1.6.0p8/check-mk-raw-1.6.0p8_0.disco_amd64.deb
		apt-key add Check_MK-pubkey.gpg
    gdebi -n check-mk-raw-1.6.0p8_0.disco_amd64.deb
		omd create monitor
		omd start monitor
    wget -q http://172.28.128.12/monitor/check_mk/agents/check-mk-agent_1.6.0p8-1_all.deb
    gdebi -n check-mk-agent_1.6.0p8-1_all.deb
		cd /omd/sites/monitor/etc
		rm htpasswd
		htpasswd -nb cmkadmin admin > htpasswd
    omd version
    whoami
    echo "===================================================================================="
                              hostnamectl status
    echo "===================================================================================="
    echo "         \   ^__^                                                                  "
    echo "          \  (oo)\_______                                                          "
    echo "             (__)\       )\/\                                                      "
    echo "                 ||----w |                                                         "
    echo "                 ||     ||                                                         "
	  SHELL
    # checkmk.vm.provision "shell", inline: $all_nodes_script, privileged: false

	end

      	config.vm.define "vg-client-01" do |client|
          client.vm.box = "bento/ubuntu-19.04"
          client.vm.hostname = "vbox-client-01"
          client.vm.network "private_network", ip:"172.28.128.15"
      	client.vm.provider "virtualbox" do |vb|
      	  vb.memory = "512"
      	end
      	client.vm.provision "shell", inline: <<-SHELL
        hostnamectl set-hostname vg-client-01
        echo "172.28.128.12 vg-checkmk-srv.local vg-checkmk-srv" | tee -a /etc/hosts
        echo "172.28.128.15 vg-client-01.local vg-client-01" | tee -a /etc/hosts
        echo "172.28.128.16 vg-client-02.local vg-client-02" | tee -a /etc/hosts
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


        config.vm.define "vg-client-02" do |client|
          client.vm.box = "bento/ubuntu-19.04"
          client.vm.hostname = "vbox-client-02"
          client.vm.network "private_network", ip:"172.28.128.16"
      	client.vm.provider "virtualbox" do |vb|
      	  vb.memory = "512"
      	end
      	client.vm.provision "shell", inline: <<-SHELL
        hostnamectl set-hostname vg-client-02
        echo "172.28.128.12 vg-checkmk-srv.local vg-checkmk-srv" | tee -a /etc/hosts
        echo "172.28.128.15 vg-client-01.local vg-client-01" | tee -a /etc/hosts
        echo "172.28.128.16 vg-client-02.local vg-client-02" | tee -a /etc/hosts
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
        client.vm.provision "shell", inline: $ubuntu_docker_script, privileged: false
        client.vm.provision "shell", inline: $ubuntu_docker_build_script, privileged: false
      	end


 end
