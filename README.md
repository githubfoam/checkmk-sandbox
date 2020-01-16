#
~~~~

  Operating System: Ubuntu 19.04

vagrant@vg-checkmk-srv:~$ gdebi -n check-mk-raw-1.6.0p8_0.xenial_amd64.deb
Reading package lists... Done
Building dependency tree
Reading state information... Done
Reading state information... Done
This package is uninstallable
Dependency is not satisfiable: libevent-1.4-2
~~~~
~~~~

vg-checkmk-srv: This package is uninstallable
vg-checkmk-srv: Dependency is not satisfiable: libperl5.22
~~~~
~~~~
  Operating System: Ubuntu 18.04
vagrant up
vagrant up vg-checkmk-srv vg-checkmk-client
vagrant destroy -f

u/p cmkadmin/admin
http://localhost:8080/monitor/check_mk/index.py
~~~~

~~~~
Installation of the Raw Edition

# docker container run -dit -p 8080:5000 --ulimit nofile=1024 --tmpfs /opt/omd/sites/cmk/tmp:uid=1000,gid=1000 -v monitoring:/omd/sites --name monitoring -v /etc/localtime:/etc/localtime:ro --restart always checkmk/check-mk-raw:1.6.0-latest

create a persistent memory and remove it automatically when the container stops,
# docker container run --rm -dit -p 8080:5000 --tmpfs /opt/omd/sites/cmk/tmp:uid=1000,gid=1000 --ulimit nofile=1024 -v /omd/sites --name monitoring -v /etc/localtime:/etc/localtime:ro checkmk/check-mk-raw:1.6.0-latest

# docker container logs monitoring
initial password for the cmkadmin account in the logs that are written for this containe

http://localhost:8080/cmk/check_mk/

https://checkmk.com/cms_introduction_docker.html

~~~~
~~~~

https://hub.docker.com/r/checkmk/check-mk-raw/

https://checkmk.com/cms_install_packages.html#Debian%20and%20Ubuntu-1
Monitoring Linux
https://checkmk.com/cms_agent_linux.html#manual
Further free plug-ins from users, partners or third parties are available at the Checkmk Exchange.
https://checkmk.com/check_mk-exchange.php
~~~~
