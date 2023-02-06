# Partició xifrada am DMCRYPT

## Primers pasos

Primer instal·lem els programes
```sh
apt install cryptsetup hashalot
```

## Més pasos

### Formatejar el dispositiu
```sh
# cryptsetup --type luks2 --cipher aes-xts-plain64 --hash sha256 --iter-time 2000 --key-size 256 --pbkdf argon2i --sector-size 512 --use-urandom --verify-passphrase luksFormat /dev/sdg
```
On /dev/sdg és el dispositiu (disc dur en aquest cas)

Ens sortirà un avís que les possibles dades del dispositiu seràn sobre-escrites, i que si hi estem d'acord que escribim en **MAJÚSCULES** yes.<br />

### Obrir el disposisiu
```sh
# cryptsetup open /dev/sdg HOLA
```
Netejem el que hi pugui haver:
```sh
# pv -tpreb /dev/zero | dd of=/dev/mapper/HOLA bs=128M
```

Creem el sistema de fitxers dins el dispositiu
```sh
# mkfs.ext4 /dev/mapper/HOLA
```

Tanquem el dispositiu per poder-lo obrir i verificar que estigui tot correcte
```sh
# cryptsetup close /dev/mapper/HOLA
```

Obrir mitjançant el navegador de fitxers gràfic i al **primer ús** canviar els permisos
```sh
# chown -R **usuari habitual** <carpeta on està muntat>
```

# Verificar slots ocupats
```sh
# cryptsetup luksDump /dev/sda2
```
