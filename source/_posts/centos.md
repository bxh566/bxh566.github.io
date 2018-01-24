---
title: centos
date: 2018-01-24 07:36:19
tags: [note]
---

# CentOS7

### BBR
(BBR)[https://www.vultr.com/docs/how-to-deploy-google-bbr-on-centos-7]
```
yum update
cat /etc/redhat-release ##check version is 7.3.1611
#update core
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install kernel-ml -y
#update grub and restart
egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'
#choose kernel 4.9, line entry from above output, start with line count 0
grub2-set-default 0
reboot
#after reboot, reconnect to server
uname -r ##check core is update to 4.9
#enable bbr
vi /etc/sysctl.conf
##add below content
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
sysctl -p #load
##check bbr
lsmod | grep bbr
```

### FireWallD
(FireWallD)[https://www.vultr.com/docs/using-firewalld-to-manage-your-firewall-on-centos-7]
1. add port
```
firewall-cmd --add-port 2124/tcp --permanent
firewall-cmd --reload
```
1. add service
```
#custom ssh service
cp /usr/lib/firewalld/services/ssh.xml /etc/firewalld/services/ssh-custom.xml
#edit ssh-custom.xml, change port

#add custom service to firewalld for public visit
firewall-cmd --reload
firewall-cmd --permanent --zone=public --add-service=ssh-custom
firewall-cmd --reload
#check if the service is added
firewall-cmd --zone=public --list-services
```

### change ssh port
(SSH port)[https://www.vultr.com/docs/changing-your-ssh-port-for-extra-security-on-centos-6-or-7]
```
vi /etc/ssh/sshd_config
##change the port in this sshd_config file
Port 2124  # the port you want to change it to
##update firewall
iptables -I INPUT -p tcp --dport 2124 --syn -j ACCEPT
service iptables save
semanage port -a -t ssh_port_t -p tcp 2124
### change as below in centos7
firewall-cmd --add-port 2124/tcp --permanent
firewall-cmd --add-port 2124/tcp
#restart SSH
service sshd restart
```

### add custom service to centos7
1. create service config file
```
vim /etc/systemd/system/myapp.service
```

1. add below content to this file
```
[Unit]
Description=myapp
[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/myapp -c /etc/myapp.conf.json
[Install]
WantedBy=multi-user.target
```

1. add service and start
```
systemctl enable myapp
systemctl start myapp
systemctl status myapp -l

```


