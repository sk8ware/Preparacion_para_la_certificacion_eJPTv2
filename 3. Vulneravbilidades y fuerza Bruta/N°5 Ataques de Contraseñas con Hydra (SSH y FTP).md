
----
- TAG: #Fuerza-Bruta #Hydra #Contraseñas #SSH #FTP #Maquina #Friendly3 
----
# Ataques de Fuerza Bruta con Hydra

Vamos a empezar a hablar sobre los ataques de fuerza bruta utilizando la herramienta **Hydra**, algo que definitivamente estará en el examen. En esta preparación, veremos diferentes escenarios en los cuales pondremos en práctica ataques con SSH, FTP, bases de datos, etc., para aprobar con éxito el examen.

## Preparación del Entorno

Vamos a practicar con una máquina que se encuentra en la página de [HackMyVM](https://hackmyvm.eu/machines/machine.php?vm=Friendly3). Van a la parte de **Machines** y buscan la máquina **Friendly3**. Una vez descargada, la importamos a VirtualBox, ya que es una máquina vulnerable, perfecta para practicar la técnica de fuerza bruta.

No olviden configurar el adaptador puente en la configuración de red. La iniciamos al igual que nuestra máquina Kali atacante.

## Identificación de Puertos Vulnerables

Cuando estemos en el examen y queramos saber qué máquinas pueden ser vulneradas por fuerza bruta, hay que realizar un escaneo con **nmap** de la siguiente manera:

```bash
nmap -p- --open -sS -sC -sV --min-rate 5000 -n -vvv -Pn 1.2.3.4 -oN escaneo
```

Recuerden que en el examen se utiliza una resolución **min rate** de 2000, ya que nos muestra un panorama más realista.

Con el resultado, veremos que tenemos abiertos el puerto 21 y el puerto 22, los cuales generalmente son atacables con fuerza bruta.

## Uso de Hydra para Ataques de Fuerza Bruta

Para este caso, puede haber ocasiones en que debemos descubrir el usuario y la contraseña o solamente la contraseña. Vamos a utilizar la herramienta **Hydra**, ya que en el examen también está permitida y la podremos utilizar. Debemos tener detectado un usuario válido para poder atacar.

Adicionalmente, vemos que se encuentra el puerto 80 donde corre el protocolo **http**, así que nos dirigimos a la página web para investigar un poco.

Al abrir la página, vemos que se carga un texto simple mencionando el nombre de **Juan**. En el examen, traten de encontrar todos los usuarios posibles, pueden estar dentro de otro puerto, en el código fuente, etc.

## Ataque de Fuerza Bruta con Hydra

Ya sabemos el usuario del protocolo **ssh** y **ftp**, pero no sabemos la clave. Aquí entra en juego **Hydra**. Empezaremos atacando el puerto 21 y luego el puerto 22.

La herramienta se utiliza de la siguiente manera:

```bash
hydra -l juan -P
```

Hay un directorio muy importante que deben saber dónde se encuentra, el **wordlists**. Por lo general, siempre se encuentra en la ruta **/usr/share/wordlists**. En el examen puede que ya venga descomprimido o no, pero si lo ven en archivo `.gz`, lo descomprimen de la siguiente manera:

```bash
gunzip rockyou.txt.gz
```

Ya sabiendo la ruta de nuestro **rockyou.txt**, vamos a completar el comando de hydra:

```bash
hydra -l juan -P /usr/share/wordlists/rockyou.txt ssh://1.2.3.4
```

Al terminar el ataque de fuerza bruta, veremos que nos muestra el usuario y la contraseña. Ahora tratamos de loguearnos por SSH con las credenciales indicadas.

- **Usuario**: juan
- **Contraseña**: alexis

Para ingresar, es tan sencillo como lo siguiente:

```bash
ssh juan@1.2.3.4
```

Ingresamos la contraseña y ¡pa' dentro!

## Ataque de Fuerza Bruta en Protocolo FTP

Si tratamos de ingresar por el protocolo **ftp**, es de la siguiente manera:

```bash
ftp 1.2.3.4
```

Nos pedirá el nombre y la contraseña. Volvemos a hacer un ataque de fuerza bruta con Hydra, pero al protocolo **ftp**:

```bash
hydra -l juan -P /usr/share/wordlists/rockyou.txt ftp://1.2.3.4
```

Y veremos que es la misma contraseña, así que con esto, igual pa' dentro 😄