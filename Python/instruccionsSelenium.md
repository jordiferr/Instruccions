# Selenium

#### Crear una pestanya

```python3
browser.execute_script("window.open('<url/>','new window')")
```

#### Llistar pestanyes (guardar a variable)

```python3
finestres = browser.window_handles
```

#### Canviar a pestanya

```python3
browser.switch_to.window(finestres[1])
```

#### Tancar la pestanya

```python3
browser.switch_to.window(finestres[1])
```
Acte seguit, indicar al selenium que volem tornar a la pestanya anterior:
```python3
browser.switch_to.window(finestres[0])
```
I a partir d'aquí podrem crear i tancar pestanyes.

#### Descarregar algun fitxer

```python3
browser.execute_script(f"location.href='url';")
```
