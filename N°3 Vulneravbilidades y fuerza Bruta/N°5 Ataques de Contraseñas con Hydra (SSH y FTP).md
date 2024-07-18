
----
- TAG: #Fuerza-Bruta #Hydra #Contraseñas #SSH #FTP 
----
Vamos a empezar hablar sobre los ataques de fuerza bruta utilizando la herramienta **Hydra**, algo que habrá en el examen si o si 

En esta preparación vamos a ver diferentes escenarios en los cuales pondremos en practica con ssh, ftp, bases de datos etc, esto para aprobar con éxito el examen.

Para ello vamos a practicar con una máquina que se encuentra dentro de la página de [HackMyVM](https://hackmyvm.eu/machines/machine.php?vm=Friendly3).
Van a la parte de **Machines** y buscamos la máquina **Friendly3**, una vez descargada la importamos a VirtualBox, ya que es una maquina vulnerable, para ser utilizada la técnica de Fuerza bruta.

No olviden configurar el adaptador puente en la configuración de red

La iniciamos al igual que nuestra máquina Kali atacante

Cuando estemos en el examen y queramos saber que máquinas pueden ser vulneradas por fuerza bruta hay que realizarlo con **nmap** de la siguiente manera

```bash
nmap -p- --open -sS -sC -sV --min-rate 5000 -n -vvv -Pn 1.2.3.4 -oN escaneo
```

Recuerden que en el examen se utiliza una resulución min rate de 2000, ya que nos muestra un panorama mas realista 

Con el resultado veremos que tenemos abiertos el puerto 21 y puerto 22, los cuales son en general atacables con fuerza bruta.

Para este caso suele ver ocasiones que debemos descubrir el usuario y contraseña o solamente la contraseña.

Vamos a utilizar la herramienta **Hydra** para este caso ya que dentro del examen tambien es permitido y lo podremos utilizar, para ello ya deberiamos tener detectado a un usuario valido para poder atacar

Adicional vemos que se encuentra el puerto 80 donde corre el protocolo **http**, asi que nos dirigimos a la pagina web para averiguar un poquito 

Al abrir la pagina vemos que se nos carga un texto simple Mencionando el nombre de **Juan**

En el examen traten de encontrar todos los usuarios posibles, pueden estar dentro de otro puerto, en el codigo fuente etc

Bueno ya sabemos el usuario del protocolo **ssh** y **ftp** pero no sabemos la clave 

Ahi entra en juego **Hydra**, empezaremos atacando el puerto 21 y luego el puerto 22

La herramienta se la utiliza de la siguiente manera 

```bash
hydra -l juan -P
```

Hay un directorio muy importante que deben saber donde se encuentra posicionada, el **wordlists**

Por lo general siempre se encuentra en la ruta **/usr/share/wordlists**, en el examen puede que ya venga descomprimido o no, pero si lo ven en archivo `.gz` lo descomprimen de la siguiente manera

```bash
gunzip rockyou.txt.gz
```

Ya sabiendo nuestra ruta de nuestro **rockyou.txt** vamos a completar el comando de hydra 

```bash
hydra -l juan -P /usr/share/wordlists/rockyou.txt ssh://1.2.3.4
```

Al terminar el ataque de feurza bruta veremos que nos muestra el usuario y contraseña 

Ahora tratamos de logearnos por ssh con las credenciales indicadas 

Usuario: **juan**

password: alexis

Para ingresar es tan sencillo como los indicare de la siguiente manera

```bash
ssh juan@1.2.3.4
```

Ingresamos la contraseña y pa dentroo men !

Ahora si tratamos de ingresar por el protocolo **ftp** es de la siguiente manera

```bash
ftp 1.2.3.4
```

Nos pedira el nombre y la contraseña

Por lo cual volvemos hacer un ataque de fuerza bruta con Hydra pero al protocolo **ftp**

```bash
hydra -l juan -P /usr/share/wordlists/rockyou.txt ftp://1.2.3.4
```

Y veremos que es la misma contraseña, asi que con esto igual pa dentro :v

