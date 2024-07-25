
---
- TAG: #Introducción #Intrusión #TTY #Stty #Export #TERM #xterm
---
# Tratamiento de TTY en una Máquina Víctima

En este apartado vamos a estar viendo algo muy directo y conciso: cómo realizar el tratamiento de la **TTY**.

Cuando obtenemos acceso a nuestra máquina víctima, al intentar hacer un `Ctrl + L` no se podrá borrar el contenido en la terminal. Si lo intentamos con `clear`, veremos que nos aparecerá un mensaje de error.

Configurar esto es muy importante ya que, cuando obtengamos acceso a alguna máquina, querremos crear archivos con `nano` y otras herramientas. Si no realizamos el tratamiento de la **TTY** será más difícil trabajar así.

El tratamiento de **TTY** siempre es el mismo:

1. Dentro de la máquina víctima, realizamos:
   ```bash
   script /dev/null -c bash
   ```
   Con esto obtendremos un prompt `www-data@simple:/$`.

2. Suspendemos la conexión con `Ctrl + Z` y, estando en la terminal, realizamos lo siguiente:
   ```bash
   stty raw -echo; fg
   ```
   Se pondrá una pantalla en negro y escribimos correctamente `reset xterm`, así tal cual como está, con el espacio y todo; si no, les saldrá un error.

3. Le damos enter y estaremos de nuevo adentro. Si intentamos hacer un `clear` veremos que sigue sin funcionar hasta que no le pasemos las dos variables que necesita.

4. Ejecutamos:
   ```bash
   export TERM=xterm
   ```
5. Y luego:
   ```bash
   export SHELL=bash
   ```

Realizando estos pasos, hemos realizado el tratamiento correcto de la **TTY**. Ahora ya estaremos mucho más cómodos dentro de nuestra máquina víctima.

En este punto ya podríamos realizar la post-explotación con técnicas como **Pivoting**, **Investigaciones**, y **Escalada de privilegios**.

Pero tranquilos que todos estos temas los estaremos viendo próximamente :D