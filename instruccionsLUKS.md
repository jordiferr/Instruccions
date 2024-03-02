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

# Canviar el nom un cop fetes totes les operacions
```sh
# cryptsetup config /dev/sdX --label <nom a mostrar>
```

# Obrir automàticament el disc només engengar

1. Generar un fitxer com a clau
	```sh
	openssl genrsa -out FITXER_RSA 4096
	```
	<br />
	o
	<br />
	```sh
	dd if=/dev/urandom of=FITXER_CLAU bs=32 count=1
	```
2. Afegir la clau al disposistiu LUKS
	```sh
	sudo cryptsetup luksAddKey /dev/sdX <camí on està el fitxer de clau>
	```
3. Utilitzar la comanda per aconseguir el UUID
	```sh
	lsblk
	```
4. Editar el fitxer /etc/crypttab afegint la següent línia
	```sh
	luks-123a45b6-bc78-091d-e2f3-ab4c5d6789d0 UUID=123a45b6-bc78-091d-e2f3-ab4c5d6789d0 <camí i fitxer amb la clau> luks
	```
5. Editar el fitxer /etc/fstab afegint la línia següent
	```sh
	/dev/mapper/luks-123a45b6-bc78-091d-e2f3-ab4c5d6789d0 <carpeta on es muntara> ext4 defaults 0 0
	```
