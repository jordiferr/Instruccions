# Partició xifrada am DMCRYPT

## Primers pasos

Primer instal·lem els programes<br />
<br />
<code>
apt install cryptsetup hashalot
<code />

## Més pasos

### Formatejar el dispositiu

<code>
\# cryptsetup --type luks2 --cipher aes-xts-plain64 --hash sha256 --iter-time 2000 --key-size 256 --pbkdf argon2i --sector-size 512 --use-urandom --verify-passphrase luksFormat /dev/sdg
<code />
<br />
On /dev/sdg és el dispositiu (disc dur en aquest cas) <br />
<br />
Ens sortirà un avís que les possibles dades del dispositiu seràn sobre-escrites, i que si hi estem d'acord que escribim en **MAJÚSCULES** yes.<br />

### Obrir el disposisiu

<code>
\# cryptsetup open /dev/sdg HOLA
<code />
<br />
Netejem el que hi pugui haver:<br />
<br />
<code>
\# pv -tpreb /dev/zero | dd of=/dev/mapper/HOLA bs=128M
<code />
<br />
Creem el sistema de fitxers dins el dispositiu<br />
<br />
<code>
\# mkfs.ext4 /dev/mapper/HOLA
<code />
<br />
Tanquem el dispositiu per poder-lo obrir i verificar que estigui tot correcte<br />

<code>
\# cryptsetup close /dev/mapper/HOLA
<code />
<br />
Obrir mitjançant el navegador de fitxers gràfic i al **primer ús** canviar els permisos<br />
<br />
<code>
\# chmod -R **usuari habitual** <carpeta on està muntat>
<code />
