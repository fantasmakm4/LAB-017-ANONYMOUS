# Explotación y escalada — LAB-017 Anonymous

## Acceso inicial al servicio

FTP aceptó el login anonymous. La carpeta /scripts permitía escritura y contenía clean.sh, removed_files.log y to_do.txt. La nota indicaba que el login anónimo debía deshabilitarse. Los cambios periódicos en removed_files.log y la posterior creación de archivos de inventario confirmaron que clean.sh se ejecutaba automáticamente.

## Contexto de usuario

La ejecución del inventario devolvió uid 1000 (namelessone). Los grupos incluían sudo y lxd. sudo -n -l pidió contraseña; no proporcionó una vía directa.

La user flag se copió a /var/ftp/scripts/user_flag.txt desde el proceso ejecutado por clean.sh y se descargó por FTP. Su valor se omite del repositorio.

## Escalada local con /usr/bin/env

La enumeración SUID encontró /usr/bin/env. Se usó el entorno privilegiado para iniciar Bash con la opción -p y copiar /root/root.txt a una ruta de FTP legible.

~~~~bash
/usr/bin/env /bin/bash -p -c "cp /root/root.txt /var/ftp/scripts/root_flag.txt && chmod 644 /var/ftp/scripts/root_flag.txt"
~~~~

El archivo resultante se descargó por FTP, confirmando lectura de root.txt mediante el binario SUID.

## Resultado y límites

- User flag recuperada.
- Root flag recuperada.
- La operación privilegiada confirmó acceso a root.txt mediante SUID env.
- No se obtuvo ni verificó una shell interactiva root. El informe no presenta una salida uid=0 que no se observó.
- Los valores de flags y los scripts temporales no se publican.

