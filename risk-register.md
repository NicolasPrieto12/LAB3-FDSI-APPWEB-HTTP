# Registro de riesgos: Laboratorio 3

Estados posibles: **Corregido**, **Mitigado**, **Aceptado**, **Pendiente para Lab 4**.

| ID | Riesgo | STRIDE relacionado | Estado | Descripcion / accion tomada |
|----|--------|---------------------|--------|-------------------------------|
| R1 | Contenido y rutas visibles en transito (HTTP sin cifrar) | Information Disclosure (H1) | Pendiente para Lab 4 | Confirmado con PCAP y ausencia de TLS. No se corrige en este lab a proposito, se resuelve con HTTPS/TLS en el Laboratorio 4. |
| R2 | Headers exponen version del servidor | Information Disclosure (H2) | **Corregido** | Antes: `Server: nginx/1.28.3 (Ubuntu)` (curl/nmap). Se aplico `server_tokens off;`. Retest: Nmap y `curl -I` ya no detectan version, header queda como `Server: nginx`. |
| R3 | Sitio sin autenticacion | (sin STRIDE especifico) | Pendiente para Lab 4 | Confirmado con `curl` devolviendo `200` sin credenciales. Se implementa identidad, sesiones y roles en el Laboratorio 4. |
| R4 | Sin correlacion de logs / repudio | Repudiation (H4) | **Mitigado** | `access.log` correlacionado: 4 a 7 peticiones 404 con User-Agent "Nmap Scripting Engine" en el mismo segundo (22:06:54) desde 192.168.0.38, mas trafico de curl y ZAP identificado. Limitacion: solo identifica IP y herramienta, no al operador humano; tambien puede dar falsos positivos con trafico legitimo repetitivo. |
| R5 | Ruta oculta accesible (ej. `.git/config`) | Information Disclosure | **Corregido** | Antes: `GET /.git/config` devolvia `200` con el contenido expuesto (se copio `.git/` al docroot para simular el riesgo real). Se aplico `location ~ /\. { deny all; }`. Retest: devuelve `403 Forbidden`. |
| R6 | Suplantacion de operador al pedir una ejecucion | Spoofing (H3) | Pendiente para Lab 4 | El `Authorization`/`requestedBy` no se valida contra un token real (la API solo esta documentada, no implementada), se corrige en Lab 4. |

## Evidencia de soporte
- Antes/despues headers: [`evidence/red/curl_headers.txt`](evidence/red/curl_headers.txt) contra [`evidence/retest/headers_after.txt`](evidence/retest/headers_after.txt)
- Antes/despues ruta oculta: [`evidence/retest/hidden_path.txt`](evidence/retest/hidden_path.txt) (403 confirmado)
- Correlacion de logs (Fase D): [`evidence/blue/access-log-nmap-detection.txt`](evidence/blue/access-log-nmap-detection.txt), capturas en [`evidence/blue/`](evidence/blue/)
- Nmap antes/despues: [`evidence/red/nmap_port80.nmap`](evidence/red/nmap_port80.nmap) contra [`evidence/retest/nmap_port80.nmap`](evidence/retest/nmap_port80.nmap)
- ZAP pasivo, 5 alertas: [`reports/zap-passive/`](reports/zap-passive/)
- Explicacion completa de como se implemento cada correccion: [`evidence/retest/comparacion-antes-despues.md`](evidence/retest/comparacion-antes-despues.md)
