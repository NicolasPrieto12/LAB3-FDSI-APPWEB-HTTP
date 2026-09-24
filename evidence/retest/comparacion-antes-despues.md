# Comparación antes/después del hardening (Paso 17)

| Prueba | Antes | Después | Evidencia |
|---|---|---|---|
| `nmap -sV -p 80` | `nginx 1.28.3 (Ubuntu)` — versión completa expuesta | `nginx` — sin versión ni distro | [nmap_port80.txt](nmap_port80.txt) vs. [../red/nmap_port80.txt](../red/nmap_port80.txt) |
| Header `Server` (curl) | `nginx/1.28.3 (Ubuntu)` | `nginx` | [headers_after.txt](headers_after.txt) |
| Headers de seguridad | Ausentes (sin `X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`) | Los 3 presentes | [headers_after.txt](headers_after.txt) |
| `curl .git/config` | `200 OK` — contenido de `.git/config` expuesto (ruta copiada al docroot para simular el riesgo) | `403 Forbidden` | [hidden_path.txt](hidden_path.txt), [git-config-403-after-hardening.png](git-config-403-after-hardening.png) |
| ZAP — "Server Leaks Version Information" | Alerta activa (Low) | Ya no debería dispararse (Server header sin versión) | [../../reports/zap-passive/](../../reports/zap-passive/) |

## Cambio aplicado en Nginx (Paso 15)
```nginx
server_tokens off;
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "DENY" always;
add_header Referrer-Policy "no-referrer" always;
autoindex off;
location ~ /\. { deny all; }
```

## Riesgos que este hardening corrige
- **R2** (headers exponen versión del servidor) → **Corregido**
- **R5** (`.git/config` accesible) → **Corregido**
- **R4** (correlación de logs) → **Mitigado** (la regla de detección de `access.log` ya identifica el patrón de escaneo; sigue teniendo falsos positivos conocidos)

Lo que este hardening **no** corrige (queda para el Laboratorio 4): **R1** (HTTP sin cifrar) y **R3** (sitio sin autenticación) — ninguno de los dos se resuelve con headers, necesitan TLS e identidad/roles respectivamente.
