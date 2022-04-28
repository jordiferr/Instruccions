# AppArmor

<strong>Totes les regles, un cop creades s'han de carregar al apparmor mitjançant:<br />

<code>
systemctl reload apparmor
<code />
<strong />

## Crear perfil

<code>
sudo app-genprof <programa\>
<code />

En un segon terminal executar el programa i, en el terminal del **genprof** prèmer la tecla **S** per a escanejar i decidir quines regles s'hauràn de complir.

## Negar tràfic a internet

Per a impedir que un programa es connecti a internet, afegir les següents línies:

<code>
deny network inet,<br />
deny network inet6,<br />
deny network raw,<br />
<code />

i, eliminar la línia:

<code>
\#include  <abstractions/base\>
<code />

si conté la línia:

<code>
\#include <tunables/global\>
<code />

a l'inici. Això és degut a que ja està inclosa dins el tunables/global
