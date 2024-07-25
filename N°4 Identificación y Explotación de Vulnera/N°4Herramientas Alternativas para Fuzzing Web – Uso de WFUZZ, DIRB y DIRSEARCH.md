
---

- TAG: #Fuzzing #Web #Wfuzz #Dirb #Dirsearch
----
# Fuzzing Web con Alternativas a Gobuster

Seguimos un poco con el **Fuzzing Web** utilizando otra alternativa para **Gobuster**. Esta herramienta nos permitía encontrar directorios dentro de una página web.

En este apartado estaremos viendo otras alternativas a esta herramienta. Esto es útil en caso de que la herramienta **gobuster** no esté instalada en la máquina o que tenga algunos errores.

Estaremos practicando con una máquina llamada [Jenk-Vulnyx](https://vulnyx.com/#jenk):
- La descargamos.
- Extraemos el fichero.
- Importamos la máquina a Virtual Box.
- Configuramos el adaptador puente en la configuración de la red.
- Iniciamos la máquina.

Desde nuestra máquina Kali Linux realizamos lo de siempre:
- `arp-scan` para localizar la máquina descargada.
- `ping` para ver si tenemos conectividad con la máquina.
- `nmap` para localizar las vulnerabilidades y puertos abiertos:
  ```bash
  nmap -p- -sS -sC -sV --open --min-rate=5000 -n -Pn -vvv 1.2.3.4 -oN escaneo
  ```

Nos dirigimos a nuestro navegador e ingresamos la dirección IP para ver lo que nos muestra.

En este punto podríamos realizar un ataque de **Fuzzing** con **gobuster** pero para esta ocasión estaremos utilizando otra herramienta.

## Wfuzz
Tenemos esta maravillosa herramienta para seleccionar el directorio que se ajuste a nuestras necesidades:
```bash
wfuzz -c --hc 404 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt http://1.2.3.4/FUZZ
```

Esta herramienta realizará **Fuzzing** como si fuera con **gobuster**, pero encontraremos los dominios al final de la **URL**.

Vemos que hay una ruta interesante, llamada **webcams**.

En esta ocasión no hizo falta utilizar el `--hl=` ya que el resultado que mostraba de líneas la mayoría resultó ser importante.

## Dirb
Ahora en este ejemplo veremos que nos mostrará muy poquitos ejemplos ya que emplea un diccionario más pequeño:
```bash
dirb http://1.2.3.4/
```

Pero no está mal utilizarlo por cualquier ocasión.

## Dirsearch
Esta herramienta también es muy útil al momento de realizar fuzzing web:
```bash
dirsearch -u http://1.2.3.4/
```

Esta herramienta es automática ya que no tenemos que pasarle ningún directorio ni nada. Empieza buscando por extensiones, métodos HTTP, threads, etc.

Si revisamos el output, nos daremos cuenta de que no nos muestra el dominio **webcams**. Eso se debe a que no hemos asignado ningún diccionario y por ende emplea uno automáticamente y mucho más corto al parecer.

Utilizamos el parámetro `-w` seguido del diccionario que hemos empleado anteriormente:
```bash
dirsearch -u http://1.2.3.4/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt
```

Y efectivamente encontraremos el directorio **webcams** utilizando esta herramienta mucho más sencilla.

Estas herramientas son otras alternativas a **gobuster**, pero tomen muy en cuenta que no es lo mismo hacer búsqueda de directorios web que es lo que hemos estado haciendo en este bloc, que hacer una búsqueda de subdominios que era lo que hacíamos con **Wfuzz** en blocs anteriores.

Como les comenté, no realizaremos la máquina completa, aunque está muy interesante, ya que solo quería mostrarles las otras opciones a **gobuster** ya que no viene mal saber estas alternativas al momento del examen. :)