# Ciència amb Python3 (Scipy, Numpy, Sympy, Matplotlib)

## Scipy

Per utilitzar les constants físiques:<br />
<br />
from scipy.constants import physical_constants as pc<br />
pc["electron mass"]<br />


## Sympy

Tot comença amb:<br />
<br />
import sympy as sp<br />
sp.init_printing( use_unicode = True )<br />
<br />
opcionalment es pot utilitzar "use_latex=True"<br />


### Resoldre inequacions

x = sp.symbols('x')<br />
sp.solveset( EQUACIO_AMB_INEGUALTAT, x, sp.Reals)<br />
<br />
on EQUACIO_AMB_INEGUALTAT pot ser: sp.Abs( x\**2 - 5\*x +3 ) <= 3<br />


### Resoldre sistemes d'equacions lineals

a, b = sp.symbols('a, b, c, x, d')<br />
<br />
f1 = 3\*a + 0\*b + 0\*c - 30<br />
f2 = 1\*a + 2\*b + 0\*c - 20<br />
f3 = 0\*a + 1\*b + 2\*c - 9<br />
f4 = a\*(c/2) + 1\*b - x<br />
<br />
resultat = sp.solvers.solve((f1, f2, f3, f4), (a, b, c, d, x))<br />


## Numpy

Tot comença amb:<br />
<br />
import numpy as np<br />
