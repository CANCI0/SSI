# Sesión 2

## Despliegue de máquinas virtuales con Docker
Primero prepare y luego build para levantar la estructura de cada lab
```sh
docker ps
```
Muestra todos los contenedores en ejecución
```sh
docker ps
```
Muestra todos los contenedores
```sh
docker ps -aq
```
Muestra los ids de todos los contenedores

```sh
docker rm
```
Borra todos los contenedores

```sh
docker stop [ID+]
```
Para los contenedores

```sh
docker stop $(docker ps -aq)
```
Para todos los contenedores

## Enumeración de metadatos con Exiftool
```sh
exiftool <FILENAME>
```

## Herramientas de OSINT
[Google Hacking](https://www.exploit-db.com/google-hacking-database)
[Shodan](https://www.shodanhq.com/)
[Censys](https://search.censys.io/)
[IP Location](https://www.iplocation.net/)
[Wayback Machine](https://archive.org/web/)

## Inspección de IPs y dominios


### Descubrimiento de subdominios
- [DNS Dumpster](https://dnsdumpster.com): Permite localizar subdominios de un dominio determinado
- [Netcraft - Search Web by Domain](https://dnsdumpster.com): Permite localizar subdominios de un dominio determinado
- [Virustotal](https://virustotal.com): Si vamos a "Buscar", damos un nombre DNS (por ejemplo, uniovi.es) y realizamos un escaneo de detección de URLs como el que vimos anteriormente, la pestaña
"Detalles" también muestra sus subdominios asociados.
- [CRT](https://crt.sh): Darle un nombre de dominio (por ejemplo, uniovi.es) también nos da subdominios
asociados.

### Analizar un rango de IPs

### Escaneos en LAN con nmap
```sh
nmap -sn 192.168.2.0/24
```
Para enumerar los Hosts vivos de la red local

## Privacidad y seguridad en la navegación

[Blacklight](https:/themarkup.org/blacklight): Escanea y descibre tecnologías específicas de seguimiento de usuarios de cualquier sitio web.