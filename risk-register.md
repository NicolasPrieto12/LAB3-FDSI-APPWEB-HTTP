# Registro de riesgos — Laboratorio 3

Estados posibles: **Corregido**, **Mitigado**, **Aceptado**, **Pendiente para Lab 4**.

| ID | Riesgo | STRIDE relacionado | Estado | Descripcion / accion tomada |
|----|--------|---------------------|--------|-------------------------------|
| R1 | Contenido y rutas visibles en transito (HTTP sin cifrar) | Information Disclosure (H1) | Pendiente para Lab 4 | Se corrige con HTTPS/TLS en el Laboratorio 4. |
| R2 | Headers exponen version del servidor | Information Disclosure (H2) | Pendiente | Se mitiga con `server_tokens off;` y headers de seguridad (Paso 15, una vez desplegado). |
| R3 | Sitio sin autenticacion | — | Pendiente para Lab 4 | Se implementa identidad, sesiones y roles en el Laboratorio 4. |
| R4 | Sin correlacion de logs / repudio | Repudiation (H3) | Pendiente | Blue Team debe correlacionar `access.log` con las acciones de Red Team durante la ronda. |
| R5 | Ruta oculta accesible (ej. `.git/config`) | Information Disclosure | Pendiente | Se valida con `location ~ /\. { deny all; }` en el retest (Paso 17). |

> Nota: los IDs y estados se actualizan con evidencia real una vez el equipo termine el despliegue en la instancia Ubuntu autorizada y se ejecuten las rondas Red/Blue.
