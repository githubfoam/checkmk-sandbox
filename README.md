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

https://checkmk.com/cms_install_packages.html#Debian%20and%20Ubuntu-1
Monitoring Linux
https://checkmk.com/cms_agent_linux.html#manual
~~~~
