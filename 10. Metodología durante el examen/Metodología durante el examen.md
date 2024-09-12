

-------
- TAG: #Metodología #Examen #eJPT #Final 
----
## Recomendaciones para Apuntes durante el Examen eJPT

Durante el examen, es fundamental mantener un registro claro y organizado de todo lo que descubras. Para ello, te recomiendo utilizar herramientas como **CherryTree** o **Obsidian**, ya que te permitirán estructurar la información de manera jerárquica y agregar capturas de pantalla, lo que facilitará el acceso rápido a los datos.

### 1. Organización de la Información

Es muy importante que vayas **documentando cada paso** que realices. Aquí tienes algunos consejos para estructurar tus notas de manera efectiva:

- **Anota todas las IPs descubiertas**: Es esencial que registres cada dirección IP que encuentres, junto con su sistema operativo, roles, y posibles servicios que estén corriendo en ellas.
  
  Ejemplo:
```
Dirección IP: 192.168.1.1  
Sistema Operativo: Linux (posible versión: Ubuntu 20.04)  
Servicios: SSH (puerto 22), HTTP (puerto 80)
```

  Esta información será útil más adelante al momento de ejecutar exploits o al hacer pivoting.

### 2. Resultados de Escaneo de Puertos y Servicios

Cuando utilices **Nmap** para hacer un escaneo de puertos y servicios, **anota los resultados** detalladamente, ya que te proporcionarán una visión clara de los posibles vectores de ataque.

- **Escaneo inicial con Nmap**: El escaneo puede revelarte puertos abiertos, versiones de software, y sistemas operativos. Asegúrate de registrar todo de forma organizada.

  Ejemplo:
```
Escaneo Nmap – Resultados
IP: 192.168.1.1
Puertos abiertos:
- 22/tcp   open   ssh     OpenSSH 7.4 (protocol 2.0)
- 80/tcp   open   http    Apache httpd 2.4.29
- 3306/tcp open   mysql   MySQL 5.7.28

Sistema Operativo detectado: Linux Kernel 4.x
Posibles vulnerabilidades: 
- SSH brute-force (user: root, password: weakpass)
- Apache HTTPD Path Traversal (CVE-XXXX-XXX)
```

- **Subdividir la información en secciones**: Cuando detectes servicios importantes como **MySQL**, **SSH** o **HTTP**, crea secciones dentro de tus apuntes para cada servicio y documenta la información específica, como posibles vulnerabilidades o exploits disponibles para esos servicios.

### 3. Estructura para Pivoting

En el momento de hacer **pivoting**, es fundamental mantener un registro claro y ordenado para no perder el hilo de la red que estás comprometiendo. Una buena práctica es:

- **Crear subnodos dentro de tu herramienta de notas**: Cuando consigas comprometer una máquina y usarla como punto de pivote, agrega un subnodo dentro de tu nota principal con toda la información relacionada a ese sistema específico. Esto te ayudará a agrupar la información relacionada con esa máquina y sus conexiones.

  Ejemplo:
```
Nodo: Máquina comprometida 192.168.1.1  
Subnodos: 
- Conexión SSH establecida
- Uso de túnel para escaneo de red interna (192.168.2.x)
- Servicios descubiertos en red interna:
- 192.168.2.10 - Microsoft Windows Server (puertos abiertos: 445, 3389)
- 192.168.2.15 - Linux Web Server (puerto 80, versión Apache 2.4.6)
```

Este nivel de detalle te permitirá **mantenerte organizado y eficiente** durante el examen, ya que sabrás exactamente qué información has encontrado y cómo utilizarla para avanzar.

### 4. Captura de Pantallas y Evidencia

No te olvides de **tomar capturas de pantalla** de comandos, resultados importantes, y sesiones establecidas, ya que puede que necesites volver a revisarlos después. Herramientas como CherryTree y Obsidian permiten insertar imágenes directamente en tus notas, lo que es excelente para documentar el proceso.

- Captura de pantalla de un **escaneo de Nmap**.
- Evidencias de compromisos exitosos (como acceso SSH o la obtención de una shell).
- Registros de comandos usados para vulnerabilidades específicas (como ejecución de exploits en Metasploit).

### 5. Comentarios Personales y Notas Adicionales

A medida que avances en el examen, **agrega comentarios personales** sobre lo que estás haciendo o lo que planeas hacer a continuación. Esto te ayudará a mantenerte enfocado y a recordar tus próximos pasos.

---


![[apuntes durante el examen 1.png]]

![[reconocimiento nmap.png]]

---
