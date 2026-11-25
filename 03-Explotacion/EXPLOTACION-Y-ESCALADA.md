# Explotación y escalada — LAB-017 Anonymous

## Acceso FTP y ejecución periódica

1. Se inició sesión FTP como anonymous.
2. Se entró en /scripts y se descargó clean.sh, removed_files.log y to_do.txt.
3. El directorio permitía escritura. Se reemplazó clean.sh; la tarea periódica lo ejecutó como namelessone.
4. Se recuperó user.txt escribiendo una copia en /var/ftp/scripts/user_flag.txt y descargándola por FTP.

## Escalada mediante SUID env

El inventario localizó /usr/bin/env con bit SUID. sudo -n -l no ofreció una regla utilizable porque pedía contraseña. Se usó env para lanzar Bash con -p y leer /root/root.txt, copiándolo a /var/ftp/scripts/root_flag.txt con permisos de lectura. Después se descargó el archivo por FTP.

~~~~bash
/usr/bin/env /bin/bash -p -c "cp /root/root.txt /var/ftp/scripts/root_flag.txt && chmod 644 /var/ftp/scripts/root_flag.txt"
~~~~

## Resultado preciso

- La user flag se recuperó.
- El contenido de root.txt se recuperó mediante una orden ejecutada con privilegios SUID.
- No se estableció una shell interactiva como root. El repositorio conserva este estado para no afirmar más de lo comprobado.

No se incluyen los valores de flags ni payloads de shell reversa fallidos.
