# Reconocimiento — LAB-017 Anonymous

## Objetivo

Identificar hosts activos, puertos, versiones y superficies FTP/SMB de la máquina TryHackMe 10.66.183.251.

## Escaneo inicial

~~~~bash
nmap --top-ports 100 -sV -sC -oN nmap-inicial.txt 10.66.183.251
~~~~

Resultado: host activo; 4 puertos abiertos entre los 100 revisados.

| Puerto | Estado | Servicio | Resultado de identificación |
|---|---|---|---|
| 21/tcp | open | FTP | vsftpd 3.0.3; login anónimo permitido |
| 22/tcp | open | SSH | OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 |
| 139/tcp | open | NetBIOS/SMB | Samba smbd 3.X–4.X |
| 445/tcp | open | SMB | Samba 4.7.6-Ubuntu |

El host SMB se identificó como ANONYMOUS, grupo WORKGROUP. La salida de Nmap también indicó SMB signing no requerido. No se usó ese dato como vector de ataque.

## Enumeración SMB

~~~~bash
smbclient -L //10.66.183.251 -N
~~~~

Recursos listados: print$ (controladores de impresora), pics (recurso de imágenes) e IPC$ (servicio IPC).

~~~~bash
smbclient //10.66.183.251/pics -N -c 'ls'
~~~~

El recurso pics contenía corgo2.jpg y puppos.jpeg. Ambos se descargaron para análisis.

~~~~bash
exiftool corgo2.jpg puppos.jpeg
steghide info corgo2.jpg
steghide info puppos.jpeg
~~~~

ExifTool mostró metadatos de imagen. Steghide no extrajo datos al probar una frase vacía; no se obtuvieron credenciales por esta vía.

## Enumeración FTP

Nmap confirmó que FTP permitía autenticación anonymous y mostró scripts como carpeta escribible. La sesión FTP mostró el banner NamelessOne's FTP Server. En /scripts se encontraron:

- clean.sh, permisos remotos -rwxr-xrwx.
- removed_files.log, cuyo tamaño aumentó mientras se observaba el servicio.
- to_do.txt, con una nota que recomienda deshabilitar el login anónimo.

## Enumeración local delegada al script

El script de inventario recogió id, sudo -n -l y find de los binarios SUID. Resultado relevante:

- uid 1000: namelessone.
- Grupos: namelessone, adm, cdrom, sudo, dip, plugdev y lxd.
- sudo: contraseña requerida.
- SUID útil para la cadena: /usr/bin/env.

Los resultados brutos están en nmap-inicial.txt y enum-suid.txt.
