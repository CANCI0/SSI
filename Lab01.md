# Sesión 1

## Bloque 1: Puesta en marcha de la MV

### Entrar en sesión y actualizar la máquina virtual

Una vez desplegada la máquina, la actualizamos con:
```sh
sudo apt update
sudo apt full-upgrade
```
Para ahorrar espacio en el disco:
```sh
sudo apt autoremove
sudo apt clean
```

### Crear un usuario adecuado

Para crear un nuevo usuario:
```sh
sudo adduser USER_NAME
```

Para añadirlo al grupo sudo:
```sh
sudo usermod -G <group1, group2...> -a <username>
```

### Cambiar entre usuarios correctamente
Para hacer login con otro usuario:
```sh
su - <username>
```

## Bloque 2: Operaciones básicas de seguridad

### Instalar un servidor OpenSSH
Instalamos el servidor OpenSSH con
```sh
sudo apt install openssh
```

Para permitir la autenticación con contraseña, cambiamos la configuración en /etc/ssh/sshd_config.

Para conectarnos por SSH desde otra máquina:
```sh
ssh <user>@<server> [-p <port>]
```

### Habilitar el firewall (ufw)

Para habiliar el firewall:
```sh
sudo ufw enable
```

Para deshabilitarlo:
```sh
sudo ufw disable
```

Para habilitar y deshabilitar tráfico por un puerto
```sh
sudo ufw allow <nº puerto>

sudo ufw deny <nº puerto>
```

Para ver las reglas activas
```sh
sudo ufw status
```

###  Evaluar la configuración de seguridad de la máquina virtual base: Lynis
Para instalar Lynis
```sh
sudo wget -O - https://packages.cisofy.com/keys/cisofy-software-public.key |sudo apt-key add -
sudo apt install apt-transport-https
echo "deb https://packages.cisofy.com/community/lynis/deb/ stable main" | sudo tee /etc/apt/sources.list.d/cisofy-lynis.list
sudo apt update
sudo apt install lynis
```

Para obtener una puntuación de seguridad del sistema
```sh
sudo lynis audit system --quick
```

### Enumeración básica con nmap
Para hacer un ping scan (sin port scan)
```sh
sudo nmap -sn <IP>
```

Para hacer un port scan a puertos específicos
```sh
sudo nmap -PS<port1, port2...> <IP>
```