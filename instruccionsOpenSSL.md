# Instruccions OpenSSL

## Extreure signatura d'un pdf

Lloc original:

https://gist.github.com/trueroad/5fc2de151f717e34b250ac99fa141604

/home/jordi/Programacio/C++/Extract\_PDF\_Signature/./extract-pdf-sig /home/jordi/Documents/Advocat/Posterior\ Juny\ 2021/csv.pdf prefix

## Veure els certificats un cop extrets

openssl pkcs7 -in prefix-69-0-sigdict-contents.p7s -inform DER -print\_certs

## Més idees

https://unix.stackexchange.com/questions/118265/how-to-encyrpt-a-message-using-someones-ssl-smime-p7s-file


