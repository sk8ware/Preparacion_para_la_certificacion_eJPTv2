
---
- TAG: #eJPT #Preparación #Examen 
---
En este repositorio vamos aprender que es una mascara de red, que tipos de red hay segun el tamaño que tengan y como poder identificar cada tipo de red 

De principio de temos 3 tipos de redes :

Tipo A

Tipo B

Tipo C - Tipo de red domestica (192.158.2.28 - 255.255.0 = /24), la mascara de red nos permite pasarnos dentro de una dirección ip que es la porción de host y la porción de red, donde `192.158.2` seria la porción de red y el `.28` sería la porción del hosts, asi que si dentro de nuestra red privada existe otros dispositivos tedran la misma porción de red pero diferente finalización de hosts.

Para saber que siguen siendo la misma porción de red esta la mascara de red donde nos indica que `255.255.255` representa a tal red y el `.0` nos muestra la posición de donde se encuentra ubicado el hosts

Dentro del examen esperen mucho este estilo de direcciones ip ya que suelen aparecer mucho, siempre deben identificar bien el tipo de mascara de red para ver a que tipo de red estamos enfrentandonos.

Dentro de la porcisión de hosts se pueden almacenar hasta 254 equipos y el utimo numero de equipo seria el broatcast donde permite la conexión dentro de todos los equipos de la red

En donde siempre suelen terminar el /24 