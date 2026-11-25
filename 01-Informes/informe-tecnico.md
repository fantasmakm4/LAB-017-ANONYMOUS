# LAB-017 — Anonymous

Objetivo: máquina de TryHackMe, IP de sesión 10.66.183.251.

Puertos: 21/FTP, 22/SSH, 139/SMB y 445/SMB. El recurso SMB identificado fue `pics`. El acceso FTP anónimo permitió escribir en `scripts`. La enumeración identificó `namelessone` y el SUID `/usr/bin/env`.

Se recuperaron las banderas de usuario y root. No se obtuvo una shell interactiva de root. Los valores de las banderas se excluyen del repositorio público.
