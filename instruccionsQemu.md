# QEmu

## Escanejar HDD amb Antivirus ISO

sudo qemu-system-x86\_64 -hda /dev/sd[abcdefg] -m 4G -boot order=dc -cdrom <Antivirus/>.iso <br />

## Iniciar una imatge CD/DVD

Utilitzem 512 Mb<br />
<br />
qemu-system-x86\_64 -boot d -cdrom <Imatge/>.iso -m 512<br />
