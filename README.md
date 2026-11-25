# FANTASMA | LAB-017 — ANONYMOUS

**Plataforma:** TryHackMe  
**Dificultad:** Fácil  
**Estado:** Banderas obtenidas; shell interactiva de root pendiente

## Resumen

Se identificaron cuatro puertos abiertos: FTP (21), SSH (22) y SMB (139 y 445). FTP anónimo permitió acceder a `scripts`; SMB expuso el recurso `pics`. La enumeración encontró el usuario `namelessone` y el binario SUID `/usr/bin/env`.

## Evidencias

El escaneo inicial y el inventario SUID están en `02-Reconocimiento`. Las banderas se guardan localmente en `05-Evidencias/privado` y no se publican.
