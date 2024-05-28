# Sesión 7: Seguridad de la red

## Bloque 1: Protección de redes de un host: Firewalls PF

### Administración de zonas con firewalld
Las interfaces de red que empiezan por br son de Docker.

Para instalar firewalld
```sh
sudo apt install firewalld firewall-config
```

Para abrir la configuración
```sh
sudo firewall-config
```

Para añadir una interfaz de red a la zona "public"
```sh
sudo firewall-cmd --zone=public --change-interface=enp0s3 --permanent
```

### Bloqueo de direcciones IP y redes con firewalld
```sh
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.72.15" destination address="192.168.72.1"'
```

### Reenvío de puertos en firewalld
```sh
sudo firewall-cmd --permanent --zone=public --add-forward-port=port=80:proto=tcp:toport=3345:toaddr=192.168.7.3
```

Para recargar el firewall
```sh
sudo firewall-cmd --reload
```

## Bloque 2: Protección de red en conexiones salientes y entrantes

### Protección de conexiones salientes: Filtreado de DNS con PiHole

Primero debemos desinstalar Apache si lo tenemos instalado
```sh
sudo apt-get remove apache2
```

Accedemos al servicio en 127.0.0.1

Cambiamos el DNS a PiHole en /etc/resolv.conf

### Filtrado avanzado de conexiones salientes con PiHole

Añadimos [esta URL](https://blocklistproject.github.io/Lists/everything.txt) a "Adlists"

Actualizamos Gravity: Tools > Update Gravity

### Protección de intentos de intrusión: Fail2Ban
Instalamos Fail2Ban
```sh
sudo apt install fail2ban
```

Podemos configurarlo en /etc/fail2ban/fail2ban.conf y /etc/fail2ban/jail.local

Algunos comandos para controlar el servicio
```sh
sudo fail2ban start
sudo fail2ban reload
sudo fail2ban stop
sudo fail2ban status
```

Algunas opciones son bantime, findtime y maxretry. Ponemos enabled = true para habilitar una jail. Hay que editar siempre jail.local, no jail.conf

Para desbanear IPs
```sh
fail2ban-client set <nombre de la jail> unbanip <ip>
```
### Protección de intentos de escaneo: PortSentry como honeypot

## Bloque 3: Detección de intrusiones

### Instalación y prueba de IDS Maltrail

```sh
sudo apt-get install git python3 python3-dev python3-pip python-is-python3 libpcap-dev build-essential procps schedtool
sudo pip3 install pcapy-ng
git clone --depth 1 https://github.com/stamparm/maltrail.git
cd maltrail
sudo python3 sensor.py
```

### Wazuh y ataques de red
