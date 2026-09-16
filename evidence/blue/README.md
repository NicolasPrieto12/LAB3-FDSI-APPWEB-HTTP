# evidence/blue

- [x] Captura de Wireshark local de prueba (`wireshark-filtro-puerto-8000.png`) — sirvio para validar que el trafico viaja en claro, pero falta repetirla contra la instancia real.
- [ ] `evidence/blue/lab3-http.pcap` filtrado (Paso 11, `tcpdump` en el servidor mientras Red Team pide `/` y `/public-inventory.txt`)
- [ ] Extractos de `access.log` / `error.log` (Paso 12)
- [ ] Salida de la regla de deteccion (Paso 13: `awk` de 404s + `grep -E 'nmap|curl|ZAP'`)
