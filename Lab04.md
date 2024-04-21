# Sesión 4

## Bloque 1: Cifrado asimétrico
Se generan dos claves: una pública y otra privada. Si quiero enviar un mensaje a otro necesito su clave pública. El mensaje se envía y la otra parte desencripta el mensaje con su clave privada. Si la otra parte envia un mensaje encriptada con la clave privada yo la puedo desencriptar con su clave pública. 

### Generar un par de claves (pública - privada)
Para generar la clave privada
```sh
openssl genrsa -out pub_pr­iv.key 4096
```

para generar la clave pública a partir de la privada
```sh
openssl genrsa -in pub_priv.key -pubout -out pub.key 4096
```

Para generar una clave privada que use criptografía de curva elíptica
```sh
openssl ecparam -name secp256r1 -genkey -out ecpriv.key
```

### Usa el cifrado simétrico para enviar mensajes confidenciales
El mensaje se cifra con la clave pública, y se descifra con la privada.

Para encriptar un mensaje 
```sh
openssl pkeyutil -encrypt -inkey public.key -pubin -in archivo.file -out cifrado.file
```

Para desencriptar un mensaje (la clave privada debe corresponder al par de la pública)
```sh
openssl pkeyutil -decrypt -inkey private.key -in cifrado.file -out descifrado.file
```

## Bloque 2: Combinando cifrado simétrico y asimétrico

### Usar un KDF como contraseña para un cifrado simétrico
Se deriva la clave que introducimos mediante un algoritmo (en este caso pbkdf2).
```sh
openssl enc -pbkdf2 -aes-128-ecb -k password -P > csim.priv
```

## Bloque 3: Integridad y firmas digitales

### Uso de cifrado autenticado

### Firmas digitales para integridad
Para firmar un archivo con la clave privada
```sh
openssl dgst -sha256 -sign private.key -out signature.data document.pdf
```

Para verificar la firma con la clave pública
```sh
openssl dgst -sha256 -verify public.key -signature signature.date original.file
```

### Firmar un PDF con un certificado autogenerado
Para crear un certificado y un par de claves
```sh
openssl req -newkey rsa:4096 -keyout private.key -out request.csr
```

Para crear un certificado a partir de una clave privada existente
```sh
openssl req -new -key private.key -out request.csr
```

Para exportar el certificado a .pfx
```sh
openssl pkcs12 -export -out certificado.pfx -inkey private.key -in request.csr
```

## Bloque 4: Los conceptos de Confidencialidad, Integridad y Autenticación (CIA) en tu día a día

### SSH sin contraseña

### Comprobación manual de la firma de un programa descargado
```sh
sha256sum <archivo>
```

### Marcas de agua

Descargamos el paquete imagemagick
```sh
sudo apt install imagemagick
```

Texto corto, marca de agua grande
```sh
convert -density 150 -fill "rgba(255,0,0,0.25)" -gravity Center -pointsize 80 -draw "rotate -45 text 0,0 <text>" <original image>
<watermarked image>
```

Marca de agua de dos líneas
```sh
convert -density 150 -fill "rgba(255,0,0,0.50)" -pointsize15 -draw "rotate -15 text 0,200 '<line of text 1>'" -draw "rotate -15 text -25,260 '<line of text 2>'" <original image> <watermarked image>
```

### Metadatos
Descargamos el paquete exiftool
```sh
sudo apt install exiftool
```

Para consultar los metadatos actuales de una imagen
```sh
exiftool <fichero de imagen>
```

Eliminar todos los metadatos de una imagen
```sh
exiftool -all= <fichero de imagen> 
```