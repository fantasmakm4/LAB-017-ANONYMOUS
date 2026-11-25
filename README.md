![Portada FANTASMA — LAB-017 Anonymous](04-Imagenes/LAB-017-ANONYMOUS.png)

# FANTASMA | LAB-017 — ANONYMOUS

**TryHackMe | FANTASMA LAB-017 ANONYMOUS | Parrot OS, Nmap, FTP anónimo escribible, SMB con recurso pics, clean.sh periódico como namelessone, enumeración SUID y /usr/bin/env con bash -p | User flag y root.txt recuperados por FTP; shell interactiva root pendiente | 4 puertos abiertos | CVE no confirmada**

**Autor:** FANTASMA  
**Plataforma:** TryHackMe  
**Máquina:** Anonymous  
**IP de sesión:** 10.66.183.251 (dinámica)  
**Sistema atacante:** Parrot Security OS  
**Dificultad:** Principiante / Fácil  
**Estado:** User flag y root.txt recuperados; shell interactiva root pendiente  
**CVE:** No confirmada

## Descripción de la portada

Infografía panorámica con el avatar FANTASMA —calavera encapuchada, cuernos, ojos rojos, alas, luna roja y portátil— sobre fondo oscuro y paneles neón cian/magenta. Los seis pasos describen el trabajo real: Nmap identifica 21/FTP, 22/SSH y 139/445/SMB; SMB expone pics; FTP anónimo permite escribir en scripts; clean.sh periódico opera como namelessone; se identifica el SUID /usr/bin/env; y bash -p permite leer root.txt. El terminal muestra uid 1000 (namelessone). El cierre declara que root.txt fue leído y que no se obtuvo una shell interactiva root. La portada muestra IP de sesión y CVE no confirmada. No expone flags ni credenciales.

## Objetivo

Enumerar la máquina, identificar la exposición de servicios, recuperar las banderas y documentar hasta dónde llegó la escalada en el laboratorio autorizado de TryHackMe.

## Resumen ejecutivo

Nmap detectó cuatro puertos abiertos. FTP permitió acceso anónimo a /scripts, una carpeta escribible que contenía clean.sh. El script se ejecutaba periódicamente como namelessone. SMB ofrecía el recurso pics con dos imágenes, que no resultaron necesarias para el compromiso. La enumeración local encontró /usr/bin/env con SUID. Se usó env para ejecutar Bash con privilegios y copiar /root/root.txt a un archivo descargable mediante FTP. Se recuperaron los contenidos de user.txt y root.txt. No se obtuvo una sesión interactiva como root, por lo que el repositorio no afirma que esa shell se haya conseguido.

## Reconocimiento de servicios

| Puerto | Servicio | Versión / observación |
|---|---|---|
| 21/tcp | FTP | vsftpd 3.0.3; acceso anónimo permitido |
| 22/tcp | SSH | OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 |
| 139/tcp | SMB | Samba smbd 3.X–4.X |
| 445/tcp | SMB | Samba 4.7.6-Ubuntu |

El escaneo y el inventario SUID completos están en 02-Reconocimiento.

## Ruta de trabajo

1. **Reconocimiento:** Nmap revisó los 100 puertos TCP más comunes y detectó FTP, SSH y SMB.
2. **Enumeración SMB:** se listaron los recursos y se encontró pics. Se descargaron corgo2.jpg y puppos.jpeg.
3. **Enumeración FTP:** el login anonymous tuvo éxito. /scripts permitía escritura y contenía clean.sh, removed_files.log y to_do.txt.
4. **Acceso de usuario:** clean.sh se ejecutaba periódicamente. La ejecución del inventario identificó uid 1000, namelessone. Se recuperó user.txt a través del directorio FTP.
5. **Escalada local:** sudo requería contraseña. La lista SUID incluía /usr/bin/env; con bash -p se leyó /root/root.txt y se copió a un archivo legible por FTP.
6. **Estado final:** ambas banderas se recuperaron; la shell interactiva root no se obtuvo.

## Hallazgos

- FTP anónimo habilitado y directorio de scripts modificable.
- El archivo clean.sh podía sustituirse por anonymous y era ejecutado periódicamente.
- SMB permitía enumerar y leer el recurso pics.
- namelessone pertenecía a los grupos adm, sudo, lxd, entre otros; sudo solicitaba contraseña.
- /usr/bin/env tenía SUID, lo que permitió ejecutar una orden con privilegios y leer root.txt.
- ExifTool mostró metadatos de las imágenes. Steghide no extrajo contenido con frase vacía; la esteganografía no formó parte de la ruta útil.
- No se confirmó una CVE aplicable.

## Herramientas utilizadas

Nmap, smbclient, cliente FTP, ExifTool, steghide, Bash y el análisis de permisos SUID.

## Documentación

- [Informe técnico](01-Informes/INFORME-TECNICO.md)
- [Reconocimiento](02-Reconocimiento/RECONOCIMIENTO.md)
- [Explotación y escalada](03-Explotacion/EXPLOTACION-Y-ESCALADA.md)
- [Descripción de la portada](04-Imagenes/README.md)
- [Evidencias públicas](05-Evidencias/publico/README.md)
- [Comandos](06-Comandos/COMANDOS.md)
- [Sala TryHackMe](https://tryhackme.com/room/anonymous)

## Recomendaciones

1. Deshabilitar FTP anónimo y eliminar la escritura para usuarios no autenticados.
2. Separar los directorios de intercambio de cualquier script ejecutado automáticamente.
3. Auditar tareas periódicas, propietarios y permisos de scripts.
4. Retirar el bit SUID de /usr/bin/env si no es necesario y revisar los binarios SUID.
5. Revisar la pertenencia de usuarios ordinarios al grupo lxd y otros grupos privilegiados.
6. No dejar archivos de banderas ni credenciales en directorios FTP públicos.

---

**FANTASMA | LAB-017 | ANONYMOUS | Banderas recuperadas; shell interactiva root pendiente**
