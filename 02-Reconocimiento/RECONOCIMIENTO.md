# Reconocimiento — LAB-017 Anonymous

## Nmap

Comando utilizado:

~~~~bash
nmap --top-ports 100 -sV -sC -oN nmap-inicial.txt 10.66.183.251
~~~~

La máquina respondió. El escaneo mostró cuatro puertos TCP abiertos:

| Puerto | Servicio |
|---|---|
| 21 | FTP |
| 22 | SSH |
| 139 | SMB / NetBIOS |
| 445 | SMB |

Versiones observadas: vsftpd 3.0.3, OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 y Samba 4.7.6-Ubuntu en 445.

## FTP

Nmap confirmó acceso anónimo y la carpeta scripts escribible. El login FTP mostró el banner “NamelessOne's FTP Server!”. En scripts se encontraron:

- clean.sh — 314 bytes en la copia original; permisos remotos -rwxr-xrwx.
- removed_files.log — registro escrito durante la ejecución periódica.
- to_do.txt — nota que recomienda deshabilitar el login anónimo.

## SMB

Listado de recursos:

~~~~bash
smbclient -L //10.66.183.251 -N
~~~~

Recursos: print$, pics e IPC$. El recurso pics contenía corgo2.jpg y puppos.jpeg. Se descargaron y revisaron con ExifTool. Steghide no extrajo datos con frase vacía.

## Cuenta y privilegios

El inventario del sistema ejecutado desde clean.sh mostró:

- uid 1000: namelessone
- Grupos: namelessone, adm, cdrom, sudo, dip, plugdev y lxd
- sudo -n -l: contraseña requerida
- SUID útil identificado: /usr/bin/env

Los resultados completos del escaneo y del inventario SUID están en este directorio.
