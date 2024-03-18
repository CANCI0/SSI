# Sesión 7: Seguridad de la red

## Protección de redes de un host: Firewalls PF

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

Para rechazar las conexiones
```sh
sudo firewall-cmd --zone=public --add-rich-rule='rule family="ipv4" source address="192.168.72.15" destination address="192.168.72.1"'
```

Para redirigir puertos
```sh
sudo firewall-cmd --permanent --zone=public --add-forward-port=port=80:proto=tcp:toport=3345:toaddr=192.168.7.3
```

Para recargar el firewall
```sh
sudo firewall-cmd --reload
```

### Bloqueo de direcciones IP y redes con firewalld

### Reenvío e puertos con firewalld

## Protección de red en conexiones salientes y entrantes

### Protección de conexiones salientes: Filtreado de DNS con PiHole

### Filtrado avanzado de conexiones salientes con PiHole

### Protección de intentos de intrusión con PiHole

### Protección de intentos de escaneo: PortSentry como honeypot

## Detección de intrusiones

### Instalación y prueba de IDS Maltrail

### Wazuh y ataques de red

## Etremando la seguridad en las conexiones VPNs

### Uso de VPNs tradicionales comerciales: ProtonVPN

### Instalar y probar ToR Browser

### Uso de VPNs P2P: ZeroTier