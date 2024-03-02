# Instruccions còpies de seguretat al núvol

## Xifrar arxius amb 7z

Si no volem complicar-nos l'existència utilitzant només el 7z / Zip / RAR n'hi ha més que suficient.

Per a més informació seguir [aquest enllaç sobre el 7zip](instruccionsVaries.md) o [aquest altre enllaç sobre el RAR](instruccionsVaries.md)

## Xifrar arixus amb GPGTAR

### Primer pas: tenir claus GPG

### GPGTAR

L'eina ***gpgtar*** permet adjuntar diversos arxius dins un únic fitxer i xifrar el fitxer resultant amb clau simètrica o amb clau pública.

```bash
gpgtar --sign --encrypt --output <SORTIDA> --recipient <recipient1> --recipient <recipient2> --recipient <recipientN> fitxer1 ... fitxerN
```

## Còpia seguretat al núvol

### Amb rclone

Degut a que la majoria de hostings només permeten un tamany reduït caldrà doncs buscar quins ens ofereixen poder allotjar còpies grans sense haver de pagar.<br />
Un d'ells és Dropbox. I permet ser utilitzat amb rclone.
