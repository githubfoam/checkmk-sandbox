#
~~~~

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
