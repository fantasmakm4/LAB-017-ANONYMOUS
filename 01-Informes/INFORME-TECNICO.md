# Informe técnico — LAB-017 Anonymous

## Ficha

- Plataforma: TryHackMe
- Máquina: Anonymous
- IP observada: 10.66.183.251 (sesión dinámica)
- Atacante: Parrot Security OS
- Resultado: user.txt y root.txt leídos; shell interactiva root no obtenida
- CVE: ninguna confirmada

## Resumen ejecutivo

La máquina expuso FTP, SSH y SMB. El servidor FTP permitió inicio anónimo; su carpeta scripts era escribible. El recurso SMB pics permitió descargar corgo2.jpg y puppos.jpeg. La ruta útil pasó por clean.sh, que se ejecutaba periódicamente como namelessone, y por el binario SUID /usr/bin/env. Con env se lanzó Bash en modo privilegiado y se copió /root/root.txt a una ruta legible por FTP. Se recuperaron ambos valores de bandera, que se mantienen fuera de Git.

## Servicios

| Puerto | Servicio | Versión / resultado |
|---|---|---|
| 21/tcp | FTP | vsftpd 3.0.3; login anónimo permitido |
| 22/tcp | SSH | OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 |
| 139/tcp | SMB | Samba smbd 3.X–4.X |
| 445/tcp | SMB | Samba 4.7.6-Ubuntu |

## Hallazgos

### H-01 — Escritura FTP anónima

La cuenta anonymous pudo acceder a /scripts. El listado mostró clean.sh con permisos de escritura para otros usuarios. La nota to_do.txt advertía que el login anónimo debía deshabilitarse.

### H-02 — Recurso SMB accesible

La enumeración SMB listó pics. Se descargaron corgo2.jpg y puppos.jpeg. ExifTool mostró metadatos de imagen; steghide no extrajo datos al probar una frase vacía. No se usaron estas imágenes para escalar.

### H-03 — Script periódico y usuario namelessone

El archivo clean.sh podía reemplazarse mediante FTP y era ejecutado por un proceso periódico. La salida de id registrada por el script identificó uid 1000 (namelessone). La cuenta tenía grupo lxd y sudo requería contraseña.

### H-04 — SUID /usr/bin/env

La enumeración de SUID encontró /usr/bin/env. Ejecutarlo para iniciar Bash con -p permitió leer /root/root.txt y copiarlo al directorio FTP. Esto confirma ejecución de una orden privilegiada. No se obtuvo una shell interactiva root.

## Cuestionario TryHackMe

- Puertos abiertos: 4
- Servicio en 21: FTP
- Servicio en 139 y 445: SMB
- Recurso compartido: pics

## Recomendaciones

1. Deshabilitar FTP anónimo y retirar permisos de escritura a usuarios no autenticados.
2. Separar los directorios de intercambio de los scripts ejecutados por tareas periódicas.
3. Auditar scripts, permisos SUID y grupos privilegiados como lxd.
4. Restringir el uso de /usr/bin/env con SUID y eliminar permisos especiales innecesarios.
5. Monitorizar cambios en scripts ejecutables y tareas programadas.
6. No publicar contraseñas ni valores de flags.

