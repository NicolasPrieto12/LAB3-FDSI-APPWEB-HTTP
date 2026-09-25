# evidence/red: Reconocimiento Red Team (completo)

Ejecutado desde Kali (192.168.0.38) contra Ubuntu + Nginx (192.168.0.6), 22-23/09/2026.

- [nmap_port80.nmap](nmap_port80.nmap) (con `.gnmap` y `.xml`): puerto 80 abierto, `nginx 1.28.3 (Ubuntu)` expuesto (evidencia R2)
- [curl_home.txt](curl_home.txt): `200 OK` sin autenticación (evidencia R3)
- [curl_headers.txt](curl_headers.txt): `404` en `/public-inventory.txt`, porque el sitio solo tiene `index.html`
- [kali-nmap-curl.png](kali-nmap-curl.png): captura de terminal con la secuencia completa
- [evidence-folder-listing.png](evidence-folder-listing.png): listado de archivos generados en Kali
- Reporte pasivo de OWASP ZAP: ver [`../../reports/zap-passive/`](../../reports/zap-passive/)
