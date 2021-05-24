# Instruccions generals

## APT

Descarregar un paquet per a poder-lo instal·lar quan l'eliminin per qualsevol tonteria:

sudo apt-get install --download-only <nom_paquet\>

## Manipulació PDFs

### QPDF

| Què fa? | Funció |
| :---    | :---   |
| Ajuntar múltiples pdf:| qpdf --empty --pages PDF_1.pdf PDF_2.pdf ... PDF_N.pdf -- FINAL.pdf |
| Extreure pàgines d'un pdf: | qpdf --empty --pages INPUT.pdf nºinici-nºfinal -- OUTPUT.pdf |
| Eliminar Streams (util per eliminar codi no desitjat) | qpdf --object-streams=disable -qdf INPUT.pdf OUTPUT.pdf |
| linearitzar PDF: | qpdf --linearize INPUT.pdf OUTPUT.pdf |
| Reparar PDF | qpdf INPUT.pdf OUTPUT.pdf |

### Okular

Útil per a passar de DJVU a PDF

# Radare2

### Funcions utils per a mi

| Funció r2 | Utilitat |
| :---     | :---      |
| axff ~str | Buscar cadenes text a una determinada funció (un cop col·locats a ella)|
| /a bytes | Cercar totes les coincidències que continguin aquells bytes |
| /R | Llista de totes les funcions que contenen un ROP |
| /R pop rdi | Llista funcions que contenen ROP RDI |

# Python3

# Instruccions aleatòries

### Wacom

Per a utilitzar la tarja Wacom a una pantalla (quan n'hi ha 2 d'instal·lades).<br />
<br />
xrandr --listactivemonitors<br />
xsetwacom --list devices	(Apuntar el ID)<br />
xsetwacom --set "19" MapToOutput HDMI-2<br />
xsetwacom --set "20" MapToOutput HDMI-2<br />
xsetwacom --set "21" MapToOutput HDMI-2<br />
xsetwacom --set "22" MapToOutput HDMI-2<br />

# Bash

#### Reanomenar fitxers en massa

Instal·lar el paquet `mmv`<br />
<br />
mmv Un\ nom\ de\ fitxer\ acanviar\*.extensio Un\ nom\ de\ fitxer\ acanviar\#1.mp4<br />

#### Enviar mail per línia de comandes

printf "COS" | mail -s "SUBJECTE" -F <mail\>

#### Capturar pantalla per vídeo (FFMPEG)

ffmpeg -y -f alsa -ac 2 -i default -acodec pcm_s16le -f x11grab -framerate 30 -video_size 1920x1080 -i :0.0+0,0 -c:v libx264 -pix_fmt yuv420p -qp 0 -preset ultrafast output.mkv<br />
(i recordar activar el dispositiu de captura de so a la icona del costat del rellotge - Monitor de Audio intern Estèreo analògic- )

#### Capturar audio (FFMPEG)

ffmpeg -f alsa -ac 2 -i default -acodec pcm_s16le <fitxer_a_guardar\>.wav

#### Encode video amb subtitols (FFMPEG)
(idea de https://askubuntu.com/questions/214199/how-do-i-add-and-or-keep-subtitles-when-converting-video)<br />
<br />
ffmpeg -i <ORIGEN\> -c:v libx264 -c:a mp3 -c:s mov_text <SORTIDA\>.mp4

#### Crear camera video /dev/video0 falsa ( fake /dev/video0 )

sudo modprobe v4l2loopback<br />
<br />
v4l2-ctl --list-devices<br />
<br />
( cara7.png es una imatge que es mostrarà com si fos la cara a mostrar. Molt útil per a pimeyes.com )<br />
ffmpeg -stream_loop -1 -re -i cara7.png -f v4l2 -vcodec rawvideo -pix_fmt yuv420p /dev/video0<br />

#### Canviar permisos

find <ruta o carpeta\> -type f -exec chmod u+rw {} \;<br />
find <ruta o carpeta\> -type f -exec chmod go-rw {} \;<br />

#### Llegir DOCX al terminal

unzip -p ./foo.docx | sed -e 's/<[^>]\{1,\}\>//g;s/<[^[:print:]]\{1,\}//g'


#### Comparar hexadecimalment dos fitxers i veure les diferències

cmp -l <fitxer1\> <fitxer2\> | gawk '{printf "%08X %02X %02X\n", $1, strtonum(0$2), strtonum(0$3)}' <br />
meld <(hexdump -C <fitxer1\>) <(hexdump -C <fitxer2\>)

### Recon

#### Informació email

curl -s emailrep.io/<EMAIL\> | jq

#### Informació IP

curl -s ipinfo.io/<IP\> | jq

### Generar cadenes contrasenyes

(longitud 15)<br />
cat /dev/urandom | strings | head -n 100 | tr -d '\n"`\\ \t' | head -c 15 && echo || tr -cd '[:alnum:] < /dev/urandom | fold -w 32 | head -n 20'


### QEmu

qemu-system-x86_64 -boot d -cdrom <imatge o /dev/cdrom> -m 512

### Squid

(https://wiki.squid-cache.org/ConfigExamples/Intercept/SslBumpExplicit)<br />
(https://techexpert.tips/squid/install-squid-with-https-ssl-decryption-ubuntu-linux/)<br />
<br />
./configure --with-default-user=proxy --with-openssl --enable-ssl-crtd<br />
make<br />
sudo make install<br />
<br />
sudo su<br />
updatedb<br />
vim /etc/ssl/openssl.cnf<br />
(Afegir les següents línies al final)

```
[ v3_ca ]

keyUsage = cRLSign, keyCertSign
```
mkdir /usr/local/squid/etc/ssl_cert -p<br />
chown proxy:proxy /usr/local/squid/etc/ssl_cert -R<br />
chmod 700 /usr/local/squid/etc/ssl_cert -R<br />
cd /usr/local/squid/etc/ssl_cert<br />

----
```
Opció 1 (openssl)

openssl req -new -newkey rsa:2048 -sha256 -days 365 -nodes -x509 -extensions v3_ca -keyout myCA.pem -out myCA.pem
(Entrar dades:
 US
 Washington
 Washington
 National Security Agency
 Unit 8200
 mamut.localhost
 email@localhost
)
openssl x509 -in myCA.pem -outform DER -out myCA.der

----
Opció 2 (certool)

certtool --generate-privkey --outfile ca-key.pem
certtool --generate-self-signed --load-privkey ca-key.pem --outfile myCA.pem

----

(Depenent de com s'hagi configurat a la instalació (--with-openssl o --with-gnutls) utilitzarem un o altre)
```

Triem openssl (el més extès)<br />
<br />
openssl req -new -newkey rsa:2048 -sha256 -days 365 -nodes -x509 -extensions v3_ca -keyout myCA.pem -out myCA.pem<br />
<br />
openssl x509 -in myCA.pem -outform DER -out myCA.der<br />
<br />
cp myCA.der /home/user/Downloads/<br />
<br />
/usr/local/squid/libexec/security_file_certgen -c -s /usr/local/squid/var/logs/ssl_db -M 4MB<br />
chown proxy:proxy /usr/local/squid/var/logs/ssl_db -R<br />
<br />
vim /usr/local/squid/etc/squid.conf<br />
(Afegir les següents línies)<br />

```
acl step1 at_step SslBump1
ssl_bump peek step1
ssl_bump bump all

http_port 3128 ssl-bump cert=/usr/local/squid/etc/ssl_cert/myCA.pem generate-host-certificates=on dynamic_cert_mem_cache_size=4MB
sslcrtd_program /usr/local/squid/libexec/security_file_certgen -s /usr/local/squid/var/logs/ssl_db -M 4MB

cache_dir ufs /usr/local/squid/var/cache/squid 1000 16 256 # 1GB as Cache

# allow all requests
acl all src 0.0.0.0/0
http_access allow all

# Make sure your custom config is befor the <ins>deny all</ins> line
http_access deny all
```
<br />
chown -R proxy:proxy /usr/local/squid -R<br />
/usr/local/squid/sbin/squid -z<br />
<br />
/usr/local/squid/sbin/squid -d 10<br />
<br />
sudo /usr/local/squid/sbin/./squid<br />
<br />

### OpenSSL

#### Extreure claus i certificats

| Què fa? | Funció |
| :---    | :---   |
| Extreure clau privada | openssl pkcs12 -in arxiu.pfx -nocerts -out clauPrivada.pem |
| Extreure certificat | openssl pkcs12 -in arxiu.pfx -clcerts -nokeys -out certificat.pem |
| Eliminar contrassenya de clau privada | openssl rsa -in clauPrivada.pem -out clauSensecontrassenya.pem |

### Vim comandes variades

| Què fa? | Comanda |
| :---    | :---    |
| Moure la última paraula al començar de la línia | :%s/^\(.\+\)\s\+\(\S\+\)$/\2 \1/ |
| Remarcar les paraules duplicades | :g/^\(.*\)$\n\1$/p |
| Eliminar N línies per sobre el patró, el patró mateix i, M línies per sota | :g/<PATRÓ\>/-N d M |

Per a reemplaçar text preservant-ne'n alguna part:<br />
<br />
```
%s/\s\+\([0-9a-f]*\)/text \1\r/g
```
<br />
| Codi | Explicació |
| :--- | :--- |
| \s\+ | Cerca un espai o més |
| \([0-9a-f]*\) | Guarda en un buffer un contingut |
| text \1\r | Escriu "text" + el contingut guardat i una nova línia |


### Wireshark

Filtres interessants per a treure porqueria:<br />
<br />
not (ip.addr == 1.1.1.1 or ip.addr == 8.8.4.4 or ip.addr == 8.8.8.8 or ip.addr == 46.24.111.140 or ip.addr == 217.182.72.0/21 or ip.addr == 172.217.0.0/16 or ip.addr == 5.9.124.96/27 or ip.addr == 52.32.0.0/11 or ip.addr == 216.58.192.0/19) and not tcp.dstport == 9090


### Comandes variades

| Comanda | Definició |
| :---    | :---      |
| mokutil | Comanda per a llistar les claus i l'estat del Secure Boot (mokutil --sb-state ; mokutil --list-enrolled) |
| osslsigncode | Comanda per a extreure i signar digitalment alguns fitxers de Windows. Útil per a bypass d'alguns anti-virus |
| lsblk -o NAME,RO | Visualitza els dispositius connectats (disc durs/dispositius de loop, etc...)  que estiguin en Read Only |
| hdparm -r 0 /dev/sdk1 | Posa a 0 el mode de només lectura |

# SSH

## Solucionar problemes GIT

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_github
```

## SSH X11 forwarding (veure programes grafics com si fos un altre usuari)

ssh -XY <altre usuari\>@localhost <programa que vols executar\>


