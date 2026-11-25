# Portada — LAB-017 Anonymous

Archivo: LAB-017-ANONYMOUS.png

Infografía panorámica FANTASMA con la calavera encapuchada, cuernos, ojos rojos, alas, luna roja y portátil. Sigue el formato visual de LAB-006: avatar prominente a la izquierda, título central, paneles numerados conectados por flechas, terminal de evidencia, lista de resultados y sello de estado.

Los seis paneles describen exactamente la ruta observada:
1. Reconocimiento con Nmap y cuatro puertos: 21 FTP, 22 SSH, 139/445 SMB.
2. Enumeración de FTP anónimo, recurso SMB pics y carpeta scripts escribible.
3. clean.sh periódico como namelessone y user flag obtenida.
4. SUID /usr/bin/env; sudo requiere contraseña.
5. bash -p y copia de root.txt por FTP.
6. root.txt leído; shell interactiva root pendiente.

El terminal muestra uid 1000 (namelessone) y la lectura de root.txt mediante env; no presenta una salida falsa uid=0. Los paneles laterales indican IP 10.66.183.251 y CVE no confirmada. No se muestran valores de flags ni credenciales.
