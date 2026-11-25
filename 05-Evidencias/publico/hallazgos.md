# Hallazgos verificados — LAB-017

- Nmap reportó cuatro puertos abiertos: 21, 22, 139 y 445.
- FTP aceptó login anonymous y expuso /scripts escribible.
- SMB listó el recurso pics con dos imágenes.
- clean.sh se ejecutaba periódicamente; el inventario identificó uid 1000 namelessone.
- sudo requería contraseña.
- /usr/bin/env aparecía entre los binarios SUID.
- User flag y root.txt se recuperaron mediante archivos descargados por FTP.
- No se obtuvo una shell interactiva root.

No se incluyen valores de las banderas ni credenciales.
