# Selenium

#### Crear una pestanya

<code>
browser.execute_script("window.open('<url/>','new window')")
<code />

#### Llistar pestanyes (guardar a variable)

<code>
finestres = browser.window_handles
<code />

#### Canviar a pestanya

<code>
browser.switch_to.window(finestres[1])
<code />

#### Tancar la pestanya

<code>
browser.switch_to.window(finestres[1])
<code />
<br />
Acte seguit, indicar al selenium que volem tornar a la pestanya anterior:
<code>
browser.switch_to.window(finestres[0])
<code />
<br />
I a partir d'aquí podrem crear i tancar pestanyes.

#### Descarregar algun fitxer

<code>
browser.execute_script(f"location.href='url';")
<code />
