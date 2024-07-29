
---
- TAG: #Inyecciones-SQL #Inyecciones #SQLmap
---
# Inyecciones SQL en el Examen eJPT

En este apartado voy a compartir un tema muy importante que dentro del examen va a venir sí o sí, y es el tema de las **Inyecciones SQL**.

Consiste prácticamente en acceder a la base de datos de la máquina víctima, de tal forma que podremos conseguir usuarios y contraseñas, información que nos permitiría acceder a la máquina por vía **SSH** u otras formas.

## Requisitos

Para poder practicar vamos a necesitar dos cosas:
- Máquina vulnerable de [Vulnyx-Máquina Shop](https://vulnyx.com/#shop)
- Máquina **Kali Linux** atacante.

## Procedimiento

Una vez iniciada, vamos a nuestra máquina **Kali Linux** y realizamos el procedimiento correcto:

1. **Arp-scan** para localizar la máquina víctima.
2. **Ping** para ver el tipo de TTL y ver a qué nos enfrentamos.
3. **Nmap** para encontrar vulnerabilidades y puertos abiertos:
   ```bash
   nmap -p- -sS -sC -sV --open --min-rate=5000 -n -Pn -vvv 1.2.3.4 -oN escaneo
   ```

Realizado el nmap, vemos que tiene el puerto **80 (HTTP)** y el puerto **22 (SSH)**, pero para ello nos hace falta un usuario y una contraseña. Recuerden que las inyecciones **SQL** nos permiten realizar eso precisamente, encontrar usuarios y contraseñas.

### Exploración de la Página Web

- Ingresamos a la dirección IP de la víctima por el navegador y vemos que parece una página sencilla de camisetas.
- Recuerden que si se nos presenta una página web, lo primero que debemos hacer es buscar directorios haciendo **Fuzzing Web**.
- Utilizamos **Gobuster** con la dirección IP:
   ```bash
   gobuster dir -u http://1.2.3.4/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
   ```
  Al ejecutarlo, encontraremos un directorio interesante llamado **/administrator**.

### Inyecciones SQL

Las **Inyecciones SQL** se realizan a través de paneles de **Login**. Si en el examen llegamos a un punto que nos pida un **Login**, podemos tratar muchas opciones, como fuerza bruta con **Hydra**, etc.

Lo más idóneo sería realizar una **inyección SQL**, ya que por este medio se puede encontrar mucha información importante.

Para realizarlo hay dos opciones:
- **De forma manual** (En el examen no viene esta parte así que nos la saltaremos).
- **De forma automática** (Viene dentro del examen), con la herramienta **SQLmap**.

---

Para utilizar la herramienta, lo pueden hacer de la siguiente manera:

```bash
sudo apt install sqlmap
```

Una vez instalada, lo podemos verificar haciendo un **sqlmap** por consola:

```bash
sqlmap
```

### Uso de SQLmap

Con esta herramienta les enseñaré cómo encontrar **bases de datos, tablas, columnas y registros**.

Es de gran ayuda si tienen un poco de conocimientos de bases de datos para poder entender mejor el tema. Les recomiendo mucho este video del profesor Mario: [Bases de datos](https://www.youtube.com/watch?v=FbGG_cyLUAc&ab_channel=ElPing%C3%BCinodeMario).

Para saber si la página web es vulnerable a inyecciones **SQL** o no, en caso de que lo sea, nos mostrará cuántas bases de datos ha encontrado:

```bash
sqlmap -u http://1.2.3.4/administrator/ -forms --dbs --batch
```

En este caso ha encontrado 4 bases de datos. Al finalizar todo, veremos que nos muestra los nombres de las 4 bases de datos, 3 de ellas siempre suelen venir por defecto:
- information_schema
- mysql
- performance_schema

En este caso hay una en particular llamada:
- Webapp

Ahora queremos saber las tablas de la base de datos **Webapp**, así que para ello debemos utilizar los siguientes parámetros:

```bash
sqlmap -u http://1.2.3.4/administrator/ --forms -D Webapp --tables --batch
```

Realizando esto, estaremos atacando a la base de datos **Webapp**. Observamos que solo encontramos una sola tabla `Users`.

Ahora la intención es estar dentro de la Tabla `Users` para ver la información dentro de dicha tabla:

```bash
sqlmap -u http://1.2.3.4/administrator/ --forms -D Webapp -T Users --columns --batch
```

Vemos que al terminar el ataque, nos indica las columnas con la información de cada una al lado derecho. De los 3 archivos que encontró, 2 vamos a escanear: `password` y `username`.

```bash
sqlmap -u http://1.2.3.4/administrator/ --forms -D Webapp -T Users -C username,password --dump --batch
```

Este comando nos permitirá ver la información de dichas **Columnas**. Al finalizar el ataque, veremos que nos encuentra 4 usuarios y 4 contraseñas, las cuales podemos ir probando una por una. En caso de que fueran miles de miles, tendríamos que crear un script para que los vaya probando uno a uno.

Encontramos que la que nos sirve es el usuario `bart` y la contraseña `b4rtpp0w4`.

### Acceso SSH

Una vez encontrado el usuario y contraseña válidos, nos dirigimos a la terminal y tratamos de logearnos por **SSH**:

```bash
ssh bart@1.2.3.4
```

Ingresamos la contraseña y veremos que nos logeamos con éxito.

### Resumen de la Inyección SQL

Prácticamente así se ejecutaría la inyección SQL para poder acceder a la base de datos utilizando la herramienta **SQLmap**. Pasos a seguir:
1. Obtener las bases de datos.
2. Obtener las tablas.
3. Obtener las columnas.
4. Obtener los registros con la información de usuarios y contraseñas para intentar ingresar por **SSH**.

En el examen nos permitirán utilizar la herramienta automatizada para obtener el acceso. Practiquen mucho esto ya que es algo que seguro vendrá en la eJPT.