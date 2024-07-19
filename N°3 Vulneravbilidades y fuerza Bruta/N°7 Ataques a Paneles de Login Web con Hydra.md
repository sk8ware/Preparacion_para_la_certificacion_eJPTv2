
----
- TAG: #Ataques #Login #Web #Hydra #Laboratorios 
----
Seguimos avanzando con el uso de **Hydra**, pero en esta ocasión ya no estaremos atacando a un puerto en especifico, sino vamos a enviar el ataque de fuerza bruta a un panel de **LOGIN**

Pero tranquilos, la sintaxis y metodología es igual como lo hemos hecho con el protocolo **ssh** y **ftp**

Cuando lo realizamos a una página web es mucho mas diferente y debemos de realizar otras cosillas como lo veremos a continuación con la utilización de **Burpsuite** para saber su inicio de **LOGIN**, para posteriormente pasar las credenciales a **Hydra** y realizar el ataque.

Para ello vamos a necesitar una maquina vulnerable de pagina de [Vulnyx (Máquina Blog)](https://vulnyx.com/#blog)

- La descargamos y extraemos el archivo
- Damo doble click al archivo `.ova`
- Terminamos la configuración 
- Configurar adaptador puente en ajuste de red
- La iniciamos y listo a hackear !

Al momento de iniciar la máquina victima suele mostrar su dirección ip y nombre, pero nosotros como buenos hackers no hacemos eso, primero la encontramos con un `arp-scan`

```bash 
arp-scan -I wlan0 --localnet
```

Recuerden cambiar el tipo de interfaz de red que tengan 

Recuerden localizarlo con el identificador de la MAC que suele empezar con 08 de VirtualBox como fabricante

En esta parte les porporcionare la pagina de login para resumir el ataque que es lo que nos importa

Ingresen al panel de login por la siguiente ruta
```web
http://1.2.3.4/my_weblog/admin.php
```

Recuerden siempre intentar con credenciales que suelen venir por defecto, como;

- Admin
- Admin

Antes de pasar cualquier información a **Hydra** debemos saber la ruta exacta para llegar a ese panel de login

Abrimos nuestro **burpsuite** para poder interceptar las peticiones **http** y conocer muchos detalle, uno de los cuales nos interesa.

Para ello ya debemos tener configurado nuestro **Burpsuite** para realizar esta intercepción por el local hosts y por el puerto 8080.

Vamos al apartado de **Proxy** y activamos el proxy para interceptar las peticiones 

Nos vamos al panel de login,a ctivamos el foxyproxy e interceptamos las peticiones enviando la solicitud como usuario **admin** y contraseña **cualquiercosa** 

Si regresamos a Burpsuite veremos que se intercepto con exito y vemos información que debemos pasarle a **Hydra** como la información **POST** y como se tramita el usuario y contraseña, pero en la parte de la contraseña se le agrega `^PASS^` ya que esa es la parte que no sabemos, seguido del mensaje de error que nos muestra en el panel de login cuando queremos ingresar con el usuario `admin`

```bash
hydra -t 64 -l admin -P /usr/share/wordlists/rockyou.txt 1.2.3.4 http-post-form "/my_weblog/admin.php:username=admin&password=^PASS^:Incorrect username or password."
```

Esperamos que procese por un momento y finalmente nos mostrará la contraseña para el usuario `admin`

Recuerden revisar si la petición es por **POST** o por **GET** para realizar bien el comando por consola

En ciertas paginas suele ser diferente la información pero igual el procedimiento 






