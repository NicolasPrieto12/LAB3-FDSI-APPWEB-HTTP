# Registro de riesgos — Laboratorio 3

Estados posibles: **Corregido**, **Mitigado**, **Aceptado**, **Pendiente para Lab 4**.

| ID | Riesgo | STRIDE relacionado | Estado | Descripcion / accion tomada |
|----|--------|---------------------|--------|-------------------------------|
| R1 | Contenido y rutas visibles en transito (HTTP sin cifrar) | Information Disclosure (H1) | Pendiente para Lab 4 | No se corrige en este lab a proposito; se resuelve con HTTPS/TLS en el Laboratorio 4. |
| R2 | Headers exponen version del servidor | Information Disclosure (H2) | **Corregido** | `server_tokens off;` aplicado. Retest: Nmap y `curl -I` ya no muestran `1.28.3 (Ubuntu)`, solo `nginx`. Ver [evidence/retest/comparacion-antes-despues.md](evidence/retest/comparacion-antes-despues.md). |
| R3 | Sitio sin autenticacion | — | Pendiente para Lab 4 | Se implementa identidad, sesiones y roles en el Laboratorio 4. |
| R4 | Sin correlacion de logs / repudio | Repudiation (H4) | **Mitigado** | Blue Team correlaciono `access.log`: identifico 7×404 de la IP de Kali en el mismo segundo con User-Agent "Nmap Scripting Engine". Sigue teniendo falsos positivos conocidos (ej. favicon.ico legitimo), por eso queda "Mitigado" y no "Corregido". Ver [evidence/blue/access-log-nmap-detection.txt](evidence/blue/access-log-nmap-detection.txt). |
| R5 | Ruta oculta accesible (ej. `.git/config`) | Information Disclosure | **Corregido** | `location ~ /\. { deny all; }` aplicado. Retest: `.git/config` paso de `200 OK` (expuesto) a `403 Forbidden`. Ver [evidence/retest/hidden_path.txt](evidence/retest/hidden_path.txt). |
| R6 | Suplantacion de operador al pedir una ejecucion | Spoofing (H3) | Pendiente para Lab 4 | El `Authorization`/`requestedBy` no se valida contra un token real (la API solo esta documentada, no implementada); se corrige en Lab 4. |

## Evidencia de correccion (Paso 17 — retest)
Ver [evidence/retest/comparacion-antes-despues.md](evidence/retest/comparacion-antes-despues.md) para la tabla completa de qué se probó antes y después del hardening, con capturas y archivos de respaldo.
