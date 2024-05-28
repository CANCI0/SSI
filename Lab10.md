# Laboratorio 10. Defendiendo aplicaciones web

## Bloque 1: Instalación de un proxy inverso

### Apache 2 instalado como un proxy inverso

Instalamos Apache 2 y su módulo de proxies

```sh
sudo apt install apache2
sudo a2enmod proxy
sudo a2enmod proxy_http
```

Añadimos las rutas y los servidores a los que queremos redirigir (/etc/apache2/sites-enabled/000-default.conf)

```html
<VirtualHost *:80>
    ...
    <Location “/eii>”>
      ProxyPass “http://http://192.168.10.2/”
      ProxyPassReverse “http://http://192.168.10.2/”
    </Location>

    <Location “/epi>”>
      ProxyPass “http://http://192.168.10.3/”
      ProxyPassReverse “http://http://192.168.10.3/”
    </Location>
</VirtualHost>
```

En este caso, la ruta /eii del servidor proxy redirige a la IP 192.168.10.2, y /epi a 192.168.10.3

## Bloque 2: Web Application Firewalls (WAFs)

### Instalación de un WAF

Vamos a instalar un WAF en el reverse proxy.

Instalamos el Web Application Firewall "mod_security2".

```sh
sudo apt install libapache2-mod-security2
```

Instalamos un OWASP ModSecurity Core Rule Set (CRS) actualizado

```sh
sudo mv /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf
git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git
cd owasp-modsecurity-crs
cp crs-setup.conf.example /etc/modsecurity/crs-setup.conf
cp -r rules/ /etc/modsecurity/
```

### WAF contra ataques web

## Bloque 3: Herramientas AST para el desarrollo de aplicaciones

### Herramienta de detección automatizada de vulnerabilidades en aplicaciones SonarQube (SAST, con módulo SCA opcional)

Ejecutamos sonarqube. Configuramos el proyecto y ejecutamos el comando maven.

Añadimos esta dependencia al pom.xml del proyecto.

```html
<plugin>
  <groupId>org.owasp</groupId>
  <artifactId>dependency-check-maven</artifactId>
  <version>6.5.2</version>
  <executions>
    <execution>
      <goals>
        <goal>check</goal>
      </goals>
    </execution>
  </executions>
</plugin>
```

En proyecto/target/dependency-check-report.html sale el análisis de las dependecias de nuestro proyecto.

### Uso de un DAST contra una aplicación
Instalamos java 21
```sh
sudo apt install openjdk-21-jdk
```

Instalamos owasp-zap
```sh
echo 'deb http://download.opensuse.org/repositories/home:/cabelo/xUbuntu_22.04/ /' | sudo tee /etc/apt/sources.list.d/home:cabelo.list
curl -fsSL https://download.opensuse.org/repositories/home:cabelo/xUbuntu_22.04/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/home_cabelo.gpg > /dev/null
sudo apt update
sudo apt install owasp-zap
```

