

----
- TAG: #Burpsuite #Modificación #Solicitudes #HTTP #Repeater
---
# Uso del Repeater en Burpsuite

En este apartado, vamos a explorar la funcionalidad del **Repeater** en Burpsuite. Este es un apartado muy útil e importante de dominar.

## Requisitos

Para esta práctica, vamos a necesitar:
1. Una máquina **Kali Linux**.
2. Máquina vulnerable de **TryHackMe** [Máquina Vulnversity](https://tryhackme.com/r/room/vulnversity).
3. Burpsuite instalado en Kali Linux.

## Configuración de Burpsuite y el Navegador

Volvemos a configurar nuestro navegador y abrimos Burpsuite.

1. **Interceptar la Petición:**
   - Interceptamos la petición en Burpsuite.
   - Buscamos la línea que contiene `filename="pwned.php"`.

## Uso del Repeater

La idea es que cuando queramos realizar varias peticiones y probar varias opciones, es útil enviar la información al **Repeater**. Podemos enviar esta petición al Repeater de dos formas:

1. Haciendo click derecho sobre la petición interceptada y seleccionando `Send to Repeater`.
2. Presionando `Ctrl + R`, lo que enviará automáticamente la información al Repeater.

### Proceso en el Repeater

1. **Abrir la Pestaña del Repeater:**
   - Una vez en el Repeater, veremos toda la información de la petición en el lado izquierdo.
   - Damos click en **Send**.

2. **Ver la Respuesta:**
   - En el lado derecho, se abrirá la respuesta en código de la página.
   - Si intentamos enviar el archivo `.php`, veremos la respuesta al final diciendo "Extension not allowed".

### Modificación y Envío de la Petición

1. **Modificar la Extensión:**
   - Una vez que hayamos encontrado la extensión correcta, regresamos al apartado del **Proxy**.
   - Cambiamos la extensión a la correcta, que en este caso sería `.phtml`.

2. **Enviar la Petición:**
   - Después de modificar la extensión, damos click en **Forward**.
   - Apagamos el interceptador **Proxy** de Burpsuite.
   - Nos dirigimos al navegador web para verificar si se ha subido correctamente el archivo.

## Resumen Visual

- **Lado Izquierdo:** Petición.
- **Lado Derecho:** Respuesta.

Recuerda siempre que el lado izquierdo muestra la petición y el lado derecho muestra la respuesta. 

Al dominar estas funciones en Burpsuite, podremos realizar múltiples pruebas y manipulaciones de peticiones, lo cual es esencial para diversas tareas en pentesting y análisis de seguridad web.
