# Primers pasos inexperts

apt install lxd lxc

sudo su<br />
lxd init<br />
(configurar)<br />
exit

$ lxc image list images : `nom de distribució`<br />
$ lxc launch images: `nom de la imatge` ( per exemple centos/8 )

$ lxc list<br />
$ lxc exec <nom de la imatge descarregada> bash

# Segons pasos experts

Info extreta de [Adictos al trabajo](https://www.adictosaltrabajo.com/2018/07/11/amaras-lxd-por-encima-de-todas-las-cosas/)<br />

sudo su

## Crear imatge i primers pasos

Descarreguem la imatge: <br />

```lxc launch ubuntu:20.04 proxy```

Executem la imatge i configurem el ssh

<code>
lxc exec proxy bash<br />
root@proxy:~# useradd -m root<br />
useradd: user 'root' already exists<br />
root@proxy:~# useradd -m master<br />
root@proxy:~# sh -c 'echo master:passwd | chpasswd'<br />
root@proxy:~# usermod -a -G lxd,sudo master<br />
root@proxy:~# usermod -s /bin/bash master<br />
root@proxy:~# sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config<br />
root@proxy:~# service ssh restart<br />
root@proxy:~# export LC_CTYPE=en_US.UTF-8<br />
root@proxy:~# export LC_ALL=en_US.UTF-8<br />
root@proxy:~# source ~/.bashrc<br />
root@proxy:~# exit
<code />

Entrem mitjançant ssh:

<code>
ssh master@10.240.165.143
<code />

usuari: **master**<br />
contrasenya: **passwd**

## Segons pasos

Instal·lem el programa **squid3** per a poder fer de transparent proxy.<br />
Instal·lem el programa **openvpn** per a utilitzar el VPN.<br />
Descarreguem la configuració del vpn i (continuarà...)


# Annex

Pàgina [Simos.info](https://blog.simos.info/how-to-use-the-lxd-proxy-device-to-map-ports-between-the-host-and-the-containers/) té més informació sobre LXC.
