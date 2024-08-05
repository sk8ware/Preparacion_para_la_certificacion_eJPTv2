
---
- TAG: #Burpsuite #Modificación #Solicitudes #HTTP 
----
# Uso de Burpsuite para Modificar Archivos en TryHackMe-Simple

En esta ocasión, vamos a seguir practicando con la máquina de **TryHackMe-Simple**. Aprovecharemos para explicar un poco más a detalle sobre Burpsuite.

## Requisitos

Para esta práctica, vamos a necesitar:
1. Una máquina **Kali Linux**.
2. Máquina vulnerable de **TryHackMe** [Máquina Vulnversity](https://tryhackme.com/r/room/vulnversity).
3. Burpsuite instalado en Kali Linux.

## Configuración de Burpsuite y el Navegador

Vamos a utilizar la herramienta `Burpsuite` para editar el contexto del archivo `.php`. Esta modificación es básica pero importante al utilizar este tipo de herramientas.

1. Dirígete a tu máquina Kali Linux y abre Burpsuite.
2. Configura tu navegador para usar Burpsuite como proxy:
   - Ve a **Configuraciones**.
   - Al final, en **Network Settings**, selecciona "Manual proxy configuration".
   - Indica tu **HTTP Proxy**: `127.0.0.1`.
   - Indica el puerto `8080`.

## Uso de Burpsuite para Interceptar y Modificar Solicitudes

Burpsuite actúa como intermediario entre la máquina atacante y la página web, interceptando solicitudes y permitiendo su modificación.

1. En Burpsuite, ve a **Proxy** y activa la intercepción.
2. Intenta cargar el archivo malicioso en la página web. Al hacer click en **Submit**, verás que la información es interceptada por Burpsuite y la petición no llega al servidor.

### Modificación del Archivo con Burpsuite

3. En Burpsuite, busca la línea que contiene `filename="reverse_shell.phtml"`.
4. Da click en `Forward`. Verás que la pantalla se limpia y puedes revisar en el navegador si aparece el mensaje **Success**. Si todo salió bien, significa que la modificación fue exitosa sin necesidad de editar la extensión en local.

### Verificación y Conexión

5. Verifica que tu archivo local sigue teniendo la extensión `.php`, ya que la modificación solo se realizó a través de Burpsuite.
6. Apaga el interceptador de Burpsuite si deseas seguir navegando sin interrupciones.
7. Dirígete al directorio `/uploads` y da click en el archivo subido como `.phtml`.
8. Pon tu terminal en escucha por el puerto **443** con netcat y verifica que obtienes la conexión:

```bash
nc -nlvp 443
```

Si todo sale bien, habrás conseguido establecer la conexión a través del archivo modificado con Burpsuite.