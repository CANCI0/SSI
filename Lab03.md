# Sesión 3

## Compartir archivos entre máquinas del lab
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

## Uso de esteganografía con steghide
Sirve para ocultar secretos en imágenes sin alterar lo que la imagen muestra.

```sh
steghide embed -cf image.jpg -ef hola.txt

steghide extract -sf image.jpg
```

## Código ofuscado para que sea difícil de entender
[Editor de JavaScript](https://js.do): Editor online de JavaScript

[Empaquetador JavaScript](http://dean.edwards.name/packer/): Permite ofuscar nuestro código JavaScript

[JS Nice](http://jsnice.org/),
[js-beautify](https://beautifier.io/)
Permiten "embellecer" código JavaScript ofuscado

## Cifrar un archivo utilizando un algoritmo de clave simétrica fuerte

Para encriptar:
```sh
openssl enc -aes-128-ecb -in encriptar.txt -out a.file -pass pass:password
```

Para desencriptar:
```sh
openssl aes-128-ecb -d -in xd.file -k password
```

