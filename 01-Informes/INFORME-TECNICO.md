# Informe técnico — LAB-017 Anonymous

## Ficha del laboratorio

- Plataforma: TryHackMe
- Máquina: Anonymous
- IP observada: 10.66.183.251 (sesión dinámica)
- Sistema atacante: Parrot Security OS
- Dificultad: principiante / fácil
- Fecha: 25/11/2026
- CVE: no confirmada
- Resultado: user.txt y root.txt recuperados mediante copias FTP; shell interactiva root pendiente

## Objetivo y alcance

Enumerar los servicios de la máquina asignada en TryHackMe, analizar los recursos FTP/SMB accesibles, identificar una ruta de acceso y evaluar la escalada de privilegios. Todas las pruebas se realizaron contra la IP del laboratorio.

## Resumen ejecutivo

El escaneo identificó cuatro puertos abiertos: FTP (21), SSH (22), SMB (139) y SMB (445). FTP aceptaba login anónimo y permitía escribir en /scripts. La carpeta contenía clean.sh, removed_files.log y to_do.txt. El registro fue creciendo mientras se observaba el servicio, lo que confirmó ejecución periódica del script. Un inventario ejecutado por el script identificó la cuenta namelessone (uid 1000), sudo con contraseña requerida y varios binarios SUID, incluido /usr/bin/env.

La cuenta obtuvo user.txt a través de una copia en el directorio FTP. Para root, se abusó del SUID de env y bash -p para copiar /root/root.txt a un archivo descargable por FTP. Esto confirma la lectura de root.txt mediante una orden privilegiada, pero no una shell interactiva root. Los valores de las dos banderas se mantienen fuera de Git.

## Servicios observados

| Puerto | Servicio | Versión / resultado |
|---|---|---|
| 21/tcp | FTP | vsftpd 3.0.3; anonymous permitido |
| 22/tcp | SSH | OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 |
| 139/tcp | SMB | Samba smbd 3.X–4.X |
| 445/tcp | SMB | Samba 4.7.6-Ubuntu |

## Hallazgos

### H-01 — FTP anónimo con escritura

La sesión anonymous fue aceptada. En la raíz FTP se encontró scripts con permisos de escritura. Dentro había clean.sh con permisos -rwxr-xrwx. to_do.txt contenía una nota que advertía que el acceso anónimo debía deshabilitarse.

**Impacto:** un usuario no autenticado podía modificar un script ejecutado periódicamente.

### H-02 — Recurso SMB pics accesible

La enumeración SMB identificó print$, pics e IPC$. En pics se encontraron corgo2.jpg y puppos.jpeg. Se descargaron para análisis; los metadatos no dieron credenciales y steghide no encontró datos al probar frase vacía.

**Impacto:** archivos del recurso estaban disponibles sin autenticación.

### H-03 — clean.sh ejecutado como namelessone

El script se ejecutaba periódicamente y pudo sustituirse mediante FTP. La ejecución de un inventario registró uid 1000, usuario namelessone. Se recuperó user.txt al colocar una copia en la carpeta FTP.

### H-04 — SUID de /usr/bin/env

El inventario de permisos especiales encontró /usr/bin/env con SUID. sudo -n -l indicó que se requería contraseña. Bash iniciado con -p mediante env conservó privilegios suficientes para leer /root/root.txt y copiarlo a la carpeta FTP.

**Estado:** root.txt fue leído; no se consiguió una shell interactiva root.

## Cuestionario TryHackMe

- Puertos abiertos: 4
- Servicio en el puerto 21: FTP
- Servicio en 139 y 445: SMB
- Recurso compartido: pics

## Recomendaciones

1. Deshabilitar FTP anónimo.
2. Denegar escritura anónima y retirar scripts ejecutables de directorios FTP.
3. Separar archivos cargados de tareas programadas.
4. Revisar el uso del bit SUID en /usr/bin/env.
5. Auditar periódicamente grupos privilegiados, incluidos lxd y sudo.
6. No publicar valores de flags ni evidencias que contengan credenciales.

