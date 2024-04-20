# Sesión 2

## Bloque 1: Exploración de contenidos web

### Metadatos de documentos
```sh
exiftool <FILENAME>
```

## Bloque 2: Buscando entre contenido expuesto y pasado

### Herramientas de OSINT
|Herramienta|Descripción
|------------|-----------
|[Google Hacking](https://www.exploit-db.com/google-hacking-database)|Puedes usar el motor de búsqueda de Google para encontrar información comprometedora de cualquier objetivo en Internet
|[Shodan](https://www.shodanhq.com/)|Puedes usar el motor de búsqueda de máquinas Shodan para averiguar información acerca de cualquier dispositivo en Internet.
|[Censys](https://search.censys.io/)|Puedes usar el motor de búsqueda de máquinas Censys y otras herramientas auxiliares para averiguar información acerca de cualquier dispositivo público en Internet
|[IP Location](https://www.iplocation.net/)|Geolocaliza cualquier dirección IP
|[Wayback Machine](https://archive.org/web/)|Puedes usar el motor de búsqueda Internet Archive (aka “Wayback Machine”) para averiguar información acerca de sitios web publicados en el pasado en muchos dominios de Internet


## Bloque 3: Inspección de IPs y dominios

### Descubrimiento de subdominios
| Herramienta                                              | Descripción                                                                                                      |
|---------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| [DNS Dumpster](https://dnsdumpster.com)                  | Permite localizar subdominios de un dominio determinado                                                           |
| [Netcraft - Search Web by Domain](https://dnsdumpster.com) | Permite localizar subdominios de un dominio determinado                                                           |
| [Virustotal](https://virustotal.com)                     | Si vamos a "Buscar", damos un nombre DNS (por ejemplo, uniovi.es) y realizamos un escaneo de detección de URLs como el que vimos anteriormente, la pestaña "Detalles" también muestra sus subdominios asociados. |
| [CRT](https://crt.sh)                                    | Darle un nombre de dominio (por ejemplo, uniovi.es) también nos da subdominios asociados.                        |

### Analizar un rango de IPs
Para dominios.com > https://whois.arin.net/
Para dominios .es > https://www.nic.es/

### Escaneos en LAN con nmap
```sh
nmap -sn 192.168.2.0/24
```
Para enumerar los Hosts vivos de la red local

## Bloque 4. Privacidad y seguridad en la navegación

### Blacklight: Inspector de privacidad de la web
[Blacklight](https:/themarkup.org/blacklight) Escanea y describe tecnologías específicas de seguimiento de usuarios de cualquier sitio web. 

### Crear un entorno de navegación seguro y verdaderamente privado en tu máquina virtual
|Herramienta|Descripción
|------------|-----------
|uBlock Origin | Bloquea anuncios
|NoScript | Bloque scripts Javascript
|Privacy Badger| Evita que las páginas web rastreen nuestro historial de navegación y creen un perfil de nuestros comportamientos de navegación

### Have I been pwnd?
https://haveibeenpwned.com/ 
Permite consultar fugas de datos de usuarios.

## ANEXO: Manejo de Docker

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