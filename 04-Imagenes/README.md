# Portada — LAB-017 Anonymous

Archivo: LAB-017-ANONYMOUS.png

Infografía panorámica en estilo cyberpunk FANTASMA, siguiendo el formato visual de los laboratorios anteriores. A la izquierda aparece la calavera encapuchada con cuernos, ojos rojos y alas frente a una luna roja, junto al portátil FANTASMA. El título central identifica LAB-017 — ANONYMOUS; la esquina superior derecha indica TryHackMe.

Seis paneles conectados resumen el trabajo real:
1. Reconocimiento con Nmap: 21 FTP, 22 SSH, 139/445 SMB.
2. Enumeración: FTP anónimo, recurso SMB pics y carpeta scripts escribible.
3. Acceso: clean.sh periódico como namelessone y user flag obtenida.
4. Privilegios: SUID /usr/bin/env; sudo requiere contraseña.
5. Root flag: bash -p y copia de root.txt por FTP.
6. Estado real: root.txt leído; shell interactiva root pendiente.

La lista de evidencias indica cuatro puertos, el recurso SMB, FTP anónimo y el SUID env. El terminal muestra uid 1000 (namelessone) y la lectura de root.txt mediante env; no presenta una falsa salida uid=0. En los paneles laterales figuran la IP de sesión 10.66.183.251 y “CVE no confirmada”. No aparecen valores de las banderas ni credenciales.
