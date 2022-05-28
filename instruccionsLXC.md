# Primers pasos inexperts

```
apt install lxd lxc

sudo su
lxd init
(configurar)
exit
```
<br />

```
$ lc image list images: `nom de distribució`
$ lxc launch images: `nom de la imatge` ( per exemple centos/8 )
$ lxc stop `nom de la imatge`
$ lxc move `nom de la imatge` `nou nom`

$ lxc list
$ lxc exec <nom de la imatge descarregada> bash
```

# Segons pasos experts

Info extreta de [Adictos al trabajo](https://www.adictosaltrabajo.com/2018/07/11/amaras-lxd-por-encima-de-todas-las-cosas/)<br />

## Crear imatge i primers pasos

Descarreguem la imatge: <br />

```lxc launch ubuntu:20.04 proxy```

Executem la imatge i configurem el ssh

```
lxc exec proxy bash
root@:~# useradd -m root
useradd: user 'root' already exists
root@:~# useradd -m master
root@:~# apt-get update ; apt install ssh ; apt install lxd
root@:~# sh -c 'echo master:passwd | chpasswd'
root@:~# usermod -a -G lxd,sudo master
root@:~# usermod -s /bin/bash master
root@:~# sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
root@:~# service ssh restart
root@:~# export LC_CTYPE=en_US.UTF-8
root@:~# export LC_ALL=en_US.UTF-8
root@:~# source ~/.bashrc
root@:~# exit
```

Entrem mitjançant ssh:

```
ssh master@10.240.165.143

usuari: **master**
contrasenya: **passwd**
```

## Segons pasos

Instal·lem el programa **squid3** per a poder fer de transparent proxy.<br />
Instal·lem el programa **openvpn** per a utilitzar el VPN.<br />
Descarreguem la configuració del vpn i (continuarà...)

# Descarregar i crear màquina virtual

```
lxc launch images:opensuse/tumbleweed/desktop-kde --vm --console=vga
```

Això permet crear una màquina virtual que podem engegar-la amb:

```
lxc start <nom> --console=vga
```

# Annex

Pàgina [Simos.info](https://blog.simos.info/how-to-use-the-lxd-proxy-device-to-map-ports-between-the-host-and-the-containers/) té més informació sobre LXC.<br />
Pàgina de [Linux Containers](https://discuss.linuxcontainers.org/t/lxd-4-4-has-been-released/8574) explicant sobre <code>--console=vga</code>
