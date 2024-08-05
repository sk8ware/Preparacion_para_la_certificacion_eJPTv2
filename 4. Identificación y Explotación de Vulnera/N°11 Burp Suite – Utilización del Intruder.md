
---
- TAG: #Burpsuite #Modificación #Solicitudes #HTTP #Intruder
---
# Uso de Intruder en Burpsuite para Ataques de Diccionarios en Peticiones HTTP

En este apartado vamos aprender a utilizar **Intruder** para realizar ataques de diccionarios en una petición **http**

Para ello seguiremos practicando con las 3 herramientas que en el bloc anterior utilizamos:

## Requisitos

Para esta práctica, vamos a necesitar:
1. Una máquina **Kali Linux**.
2. Máquina vulnerable de **TryHackMe** [Máquina Vulnversity](https://tryhackme.com/r/room/vulnversity).
3. Burpsuite instalado en Kali Linux.

Esta máquina es perfecta para seguir practicando un poco con la herramienta de **burpsuite** 

## Configuración de Burpsuite y el Navegador

Ahora nos dirigimos a nuestro navegador y realizamos la configuración de red para escuchar por burpsuite, pero si no lo quieren estar haciendo, en proximas clases estare explicando como **FoxyProxy**

1. Configuramos la conexión por el puerto 8080 del navegador
2. abrimos burpsuite y nos ponemos en escucha 
3. Interceptamos la petición y con la información que vemos trataremos de simular que no sabemos cual es la extensión a utilzar para el bypass. 

Existe un la lista de extensiones que se puede ir probando linea a linea hasta ver cual acierta.

## Uso del Intruder

Ahora nos dirigimos al **Intruder** lo cual lo podemos hacer de igual de dos maneras:

1. Haciendo click derecho sobre la petición interceptada y seleccionando `Send to Intruder`.
2. Presionando `Ctrl + I`, lo que enviará automáticamente la información al Intruder.

Una vez dentro del **Intruder** seleccionamos toda la información y seleccionamos el boton de la derecha que dice "Clear&"

Ahora podemos seleccionar cualquier parte que queramos cambiar la petición hasta encontrar la correcta
### Creación de un Archivo de Extensiones
Ahora crearemos un archivo `extensiones.txt` con todos los tipos de extensiones que existen

```nano
php
phpl
py
sh
txt
html
phtml
test
```

### Configuración de Payloads en Burpsuite

Ahora haremos que burpsuite lea este archivo uno a uno hasta encontrar el correcto

Asi que regresamos a **burpsuite** y seleccionamos la parte que queremos que vaya probando uno por uno, una vez hecho eso vamos al lado derecho y damos click en **Add&**

Ahora nos dirigimos al apartado de **Settings** y en el apartado de **Grep - Extract**, este apartado nos permite que burpsuite nos muestre cuando un codigo fue exitoso o no 

1. Damos click en **Add**
2. Si la pantalla les aparece en balnco delen click en **Refetch response** para que les salga el codigo de respuesta de la página
3. Seleccionamos el mensaje de error que nos aparece y veremos que *burpsuite* automaticamente lo detectara en la parte de arriba mostrara `/form>\n(.*?)</body>`
4. Le damos en `OK`
5. Se guardará la petición para burpsuite para saber si una petición fue exitosa o no 


Ahora nos dirigimos al apartado de **Payloads** para poder agregar el diccionario que habiamos creado `extensiones.txt`

1. Damos click en **Load**, se nos cargara todas las extensiones dentro del archivo `.txt`
2. Damos click en **Start attack** en la parte superior derecha 
3. Se abrira una ventana intentando con todas las extensiones y veremos que en una nos muestra **Success**
![[burpsuite.png]]

Este ejemplo nos permite realizar en cualquier petición **HTTP** 
# Realizando Fuzzing Web con Burpsuite

Imaginen que no tenemos al alcance ninguna herramienta como **Gobuster** o **Wfuzz** ni tampoco **Dir**

Con burpsuite tambien podemos hacer **Fuzzing Web**, les mostrare un ejemplo a continuación:

1. Activamos Burpsuite para que intercepte las peticiones en **On**
2. Recargamos a la pagina web con la dirección ip `http://10.10.10.10:3333`
3. En la parte de GET / HTTP/1.1 despues del GET debe quedar asi :
```bash
GET /test HTTP/1.1
```

esto permitira que en la parte de **test** nos permita ir probando uno por uno el diccionario 

4. Lo enviamos al Intruder y seleccionamos la parte de `test` y a la derecha en el boton `Add&` le damos click
5. Vamos **Settings** y la opción de seleccionar el apartado donde sale error es opcional
6. Vamos directo a **Payloads**, agregamos el diccionario en **Payload settings-[Simple list]** o tambien tenemos la opción de añadirlos manualmente uno por uno 
(No soporta diccionarios muy grandes como el **Rockyou**) tendriamos que implementar diccionarios mas pequeños 

Agregamos manualmente estos para agilizar la explicación:
```bash
prueba
uploads
test
internal
admin
```

7. Damos click en el boton de **Start attack**
Veremos que nos muestra por codigos de estamos y en el directorio **Internal** cambia el codigo de estado a `301` 

(En caso de que seleccionen un diccionario grande siempre pueden filtrar dando click en **Status Code**)

![[burpsuite1.png]]
Veremos que se sustituye donde escribimos **test**

# Tip Final

Con esta herramienta podemos hacer el mismo procedimiento para contraseñas es decir cualquier parte de la petición **HTTP** podemos aplicar un ataque de modo Intruder y aplicar cada sustitución 

Como tip en el examen no se suele utilizar mucho burpsuite pero viene muy bien aprender un poco para cualquier caso 