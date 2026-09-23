# Registro de riesgos — Laboratorio 3

Estados posibles: **Corregido**, **Mitigado**, **Aceptado**, **Pendiente para Lab 4**.

| ID | Riesgo | STRIDE relacionado | Estado | Descripcion / accion tomada |
|----|--------|---------------------|--------|-------------------------------|
| R1 | Contenido y rutas visibles en transito (HTTP sin cifrar) | Information Disclosure (H1) | Pendiente para Lab 4 | Confirmo con PCAP y ausencia de TLS. Se corrige con HTTPS/TLS en el Laboratorio 4. |
| R2 | Headers exponen version del servidor | Information Disclosure (H2) | Corregido | Antes: 'Server: nginx/1.28.3 (Ubuntu)' (curl/nmap). Se aplicó 'server_tokens off;'. Retest: nmap ya no detecta versión, header queda como 'Server: nginx'. |
| R3 | Sitio sin autenticacion | — | Pendiente para Lab 4 | Confirmado con 'curl' devolviendo '200' sin credenciales. Se implementa identidad, sesiones y roles en el laboratorio 4. |
| R4 | Sin correlacion de logs / repudio | Repudiation (H3) | Mitigado | 'Access.log' correlacionado: 4 peticiones 404 de Nmap Scripting Engine en el mismo segundo (22:06:54) desde 192.168.0.38, más tráfico de curl y ZAP identificando por User-Agent. Limitación: no identificada al operador humano, solo la IP/herramienta. |
| R5 | Ruta oculta accesible (ej. `.git/config`) | Information Disclosure | Corregido | Antes: 'GET /.git/config' devolvia '200' con el contenido. Se aplicó 'location ~ /\ { deny all: }' Retest: devuelve '403 Forbidden'. |

## Evidencia de soporte
- Antes/despúes headers: 'evidence/red/curl_headers.txt' vs 'evidence/retest/headers_after.txt'
- Antes/despúes ruta oculta: 'evidence/retest/hidden_path.txt' (403 confirmado)
- Correlación de logs: 'access.log.1' (Fase D), capturas en 'evidence/blue/'
- Nmap antes/despues: 'evidence/red/nmap_port80.*' vs 'evidence/retest/nmap_port80.*'
- ZAP pasivo (5 alertas): 'evidence/red/zap/Report.html'

