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
~~~~

## Identidad, sudo y SUID

El inventario local de la máquina registró:

~~~~bash
id
sudo -n -l
find / -type f -perm -4000 2>/dev/null
~~~~

Resultado relevante: uid 1000 namelessone; sudo requería contraseña; /usr/bin/env tenía SUID.

## Lectura de root.txt

La copia de root.txt se hizo a través de /usr/bin/env y Bash privilegiado. El valor no se reproduce aquí. No se obtuvo una shell interactiva root.
