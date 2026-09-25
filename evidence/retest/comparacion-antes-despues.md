# Comparación antes/después del hardening (Paso 17)

## R2: la versión del servidor estaba expuesta

Antes, tanto Nmap como el header `Server` de curl mostraban `nginx/1.28.3 (Ubuntu)`. Eso le regala a cualquiera que escanee el sitio exactamente qué versión de Nginx corre, y con eso podría buscar directamente qué vulnerabilidades conocidas tiene esa versión en particular.

Se implementó agregando la línea `server_tokens off;` dentro del bloque `server` de Nginx. Con esto Nginx deja de mandar su número de versión y sistema operativo en el header `Server`, y solo manda el nombre genérico "nginx".

Después del cambio: Nmap y `curl -I` ya no detectan ninguna versión, el header queda simplemente como `Server: nginx`. Evidencia en [headers_after.txt](headers_after.txt) y [nmap_port80.nmap](nmap_port80.nmap), comparado contra [../red/nmap_port80.nmap](../red/nmap_port80.nmap) (el "antes").

## Headers de seguridad ausentes

Antes del hardening, ZAP marcó como alertas la falta de `X-Content-Type-Options`, `X-Frame-Options` y `Referrer-Policy`. Sin esos headers, el navegador de quien visita el sitio queda más expuesto a ataques como clickjacking (que otro sitio lo cargue dentro de un iframe) o que el navegador intente adivinar el tipo de contenido de un archivo (MIME sniffing) y lo interprete mal.

Se agregaron esas tres líneas con `add_header` en el bloque `server` de Nginx, cada una forzando el comportamiento correcto del navegador.

Después: los tres headers ya aparecen en cada respuesta. Ver [headers_after.txt](headers_after.txt).

## R5: la carpeta .git quedó accesible

Para simular este riesgo a propósito, copiamos la carpeta `.git/` al docroot del sitio. Con eso, pedir `GET /.git/config` devolvía `200 OK` y mostraba el contenido completo del archivo de configuración de Git, algo que en un despliegue real podría filtrar información sensible del repositorio.

Se corrigió agregando la regla `location ~ /\. { deny all; }`, que le dice a Nginx que bloquee cualquier ruta que empiece con un punto (como `.git`, `.env`, etc.) sin importar qué archivo sea.

Después: la misma petición a `.git/config` ahora responde `403 Forbidden`. Ver [hidden_path.txt](hidden_path.txt) y la captura [git-config-403-after-hardening.png](git-config-403-after-hardening.png).

## Configuración completa aplicada (Paso 15)

```nginx
server_tokens off;
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "DENY" always;
add_header Referrer-Policy "no-referrer" always;
autoindex off;
location ~ /\. { deny all; }
```

## Resumen de qué quedó corregido y qué no

| Riesgo | Estado |
|---|---|
| R2, headers exponen versión del servidor | Corregido |
| R5, `.git/config` accesible | Corregido |
| R4, correlación de logs | Mitigado (la regla de detección de `access.log` ya identifica el patrón de escaneo, pero todavía puede dar falsos positivos) |
| R1, HTTP sin cifrar | Sigue pendiente a propósito, se resuelve en el Laboratorio 4 con HTTPS/TLS |
| R3, sitio sin autenticación | Sigue pendiente a propósito, se resuelve en el Laboratorio 4 con identidad y roles |

Ninguno de los dos últimos se puede arreglar solo con headers de Nginx: uno necesita cifrado de transporte y el otro necesita lógica de autenticación real, cosas que quedan explícitamente fuera del alcance de este laboratorio.
