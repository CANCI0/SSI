# Laboratorio 5. Seguridad de un SO Linux (no-automatizable)

Para ponerle requisitos a las contraseñas, en /etc/pam.d/common-password

- retry=N: Permite al usuario intentar N veces introducir una contraseña que cumpla con los requisitos.
- dcredit=-N: Requiere al menos N dígitos en la contraseña.
- ucredit=-N: Requiere al menos N letras mayúsculas.
- lcredit=-N: Requiere al menos N letras minúsculas.
- ocredit=-N: Requiere al menos N caracteres no alfanuméricos.

```sh
password requisite pam_cracklib.so retry=3 minlen=12 dcredit=-1 ucredit=-1 lcredit=-1 credit=-1

#En esta línea se añaden
password	[success=1 default=ignore]	pam_unix.so obscure yescrypt minlen=12
```