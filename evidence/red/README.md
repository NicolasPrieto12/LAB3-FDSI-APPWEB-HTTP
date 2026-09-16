# evidence/red — pendiente

Falta correr esto contra la instancia Ubuntu ya desplegada (Fase C del lab):

- [ ] `nmap -Pn -sV -p 80 "$TARGET_IP" -oA evidence/red/nmap_port80`
- [ ] `curl -i "$TARGET_URL/" | tee evidence/red/curl_home.txt`
- [ ] `curl -I "$TARGET_URL/public-inventory.txt" | tee evidence/red/curl_headers.txt`
- [ ] Reporte HTML exportado de OWASP ZAP (Manual Explore, sin Active Scan) en `reports/zap-passive/`
