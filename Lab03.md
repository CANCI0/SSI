# Sesión 3

## Bloque 1: Técnicas de no cifrado. Ocultación de la información 

### Probar los mecanismos de codificación con CyberChef

https://gchq.github.io/CyberChef/

### Uso de esteganografía con steghide
Sirve para ocultar secretos en imágenes sin alterar lo que la imagen muestra.

```sh
steghide embed -cf image.jpg -ef hola.txt

steghide extract -sf image.jpg
```

### Código ofuscado para que sea difícil de entender
| Nombre                   | Descripción                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| [Editor de JavaScript](https://js.do)       | Editor online de JavaScript                                                   |
| [Empaquetador JavaScript](http://dean.edwards.name/packer/) | Permite eliminar las tabulaciones y espacios en nuestro JS                   |
| [JS Nice](http://jsnice.org/)                | Herramienta para "embellecer" código JavaScript ofuscado                     |
| [js-beautify](https://beautifier.io/)        | Herramienta para "embellecer" código JavaScript ofuscado                     |

## Bloque 2: Criptografía Simétrica

### Cifrar un archivo utilizando un algoritmo de clave simétrica fuerte

Para encriptar:
```sh
openssl enc -aes-128-ecb -in encriptar.txt -out a.file -pass pass:password
```

## Descifrar un archivo que utiliza cifrado simétrico

Para desencriptar:
```sh
openssl aes-128-ecb -d -in xd.file -k password
```

## Probar los modos de cifrado ECB y CBC del algoritmo simétrico robusto de cifrado AES
Vamos a cifrar una imágen mediante el algoritmo ECB y el CBC. En primer lugar pasamos la foto a mapa de píxeles (.ppm). Para ello instalamos la siguiente librería:
```sh
sudo apt install graphicsmagick-imagemagick-compat
```
Para convertir la imagen a .ppm:
```sh
convert <nombre de la imagen>.png <nombre de la imagen>.ppm
```

Separamos la cabecera del cuerpo de la imagen
```sh
head -n 3 <nombre de la imagen>.ppm > cabecera.txt
tail -n +4 <nombre de la imagen>.ppm > cuerpo.bin
```

Ciframos el cuerpo
```sh
openssl enc -aes-128-ecb -in <foto>.bin -out <foto_cifrada>.bin
```

Y volvemos a construir la imagen
```sh
cat cabecera.txt <fichero con el cuerpo cifrado> > <nombre de la unión>.ppm
```

Sin embargo, este método de encriptación no es el ideal para imagenes, pues se siguen marcando la silueta de la imagen. Esto es porque el algoritmo ECB cada bloque de manera independiente, por lo que varios bloques de datos idénticos cifrados dan la misma salida. Para arreglar esto, vamos a cifrar el cuerpo de la imagen con el algoritmo CBC.

Ciframos el cuerpo
```sh
openssl enc -aes-128-cbc -in cuerpo.bin -out cuerpo_cifrado.bin -iv $(openssl rand -hex 16)
```

Lo unimos a la cabecera como antes. Ahora sí que no se reconoce la silueta 

## Cifrado de discos
Instalamos el software de cifrado de discos
```sh
sudo apt install ecryptfs-utils
```
Montamos la carpeta con el sistema de ficheros propio de la librería 
```sh
mount -t ecryptfs <tu ruta de carpeta> <tu ruta de carpeta>
```
Elegimos cifrado AES, 32 bytes de clave, plaintext passthrough y filename encryption a false.

Para desmontar
```sh
sudo umount <tu ruta de carpeta>
```

## Bloque 3. Introducción al cracking de contraseñas sin conexión

### John the Ripper para romper OpenSSL

Instalamos John The Ripper
```sh
sudo apt install john
```
Encriptamos un archivo para probar a desencriptarlo
```sh
openssl enc -aes-128-cbc -in <input> -out <output>
```

Lo pasamos a formato john
```sh
openssl2john -c 1 <output>
```

Lo tratamos de desencriptar
```sh

```

### John the Ripper + contraseñas de usuario + generador de listas de palabras crunch

Con la herramienta unshadow combinamos el archivo passwd con shadow
```sh
sudo unshadow /etc/passwd /etc/shadow > unshadow.txt
```

Generamos un diccionario de palabras con crunch. En este caso sabemos que la contraseña de un usuario empieza por "test", acaba con "..." y tiene tres números en medio. 
- %: Cualquier número
- @: Cualquier caracter
```sh
crunch 10 10 -t test%%%...  -o passwords.txt
```

Y por último, le pasamos el diccionario al john y el unshadow
```sh
john --wordlist=passwords.txt unshadow.txt
```

### "Fuerza bruta pura" con John the Ripper

Si queremos probar con todas las combinaciones de caracteres
```sh
john --incremental --users=<user1,user2...> --max-length=n shadow.txt
```

## ANEXO: Compartir archivos entre máquinas del lab
Podemos transportar archivos de una máquina a otra con el siguiente comando
```sh
scp ssiuser@192.168.3.2:~/Images/foto.png .
```

Si nos metemos en el directorio volume_data veremos una carpeta por cada máquina. Si mtemos un archivo ahí se mete en la carpeta /shared de la máquina.

En /etc/passwd se almacenan todos los usuarios del sistema
```sh
root:x:0:0:root:/root:/bin/bash
```
Su formato es usuario:contraseña(x si está encriptada):permiso de usuario:permiso de grupo

Las contraseñas están en /etc/shadow. Entre el primer y segundo dolar está el algoritmo de encriptación ($x$). Entre el tercer y el cuarto dolar están los caracteres aleatorios que se le añaden a la contraseña para calcular el hash. Lo que le sigue al tercer dolar es la contraseña con los caracteres hasheados.