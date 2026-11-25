# Hallazgos verificados

- Nmap reportó cuatro puertos abiertos: 21, 22, 139 y 445.
- FTP permitió login anónimo y mostró /scripts escribible.
- SMB listó el recurso pics.
- La cuenta identificada por el inventario fue namelessone (uid 1000).
- sudo requería contraseña.
- La enumeración halló /usr/bin/env con SUID.
- Se descargaron user.txt y root.txt desde copias colocadas en el directorio FTP mediante el flujo periódico y el uso de env privilegiado.
- No se obtuvo shell interactiva root.

No se incluyen los valores de las banderas.
