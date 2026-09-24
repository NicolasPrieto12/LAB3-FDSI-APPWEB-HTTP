# evidence/blue — Detección y correlación Blue Team (completo)

Ejecutado en Ubuntu (192.168.0.6), revisando `/var/log/nginx/access.log.1`, 22-23/09/2026.

- [access-log-nmap-detection.txt](access-log-nmap-detection.txt) — líneas del log + regla de detección (`grep 404 | awk | sort | uniq -c`): la IP de Kali generó 7×404 en el mismo segundo, identificada por su User-Agent "Nmap Scripting Engine"
- [access-log-nmap-detection-1.png](access-log-nmap-detection-1.png), [access-log-nmap-detection-2.png](access-log-nmap-detection-2.png) — capturas de terminal
- [ubuntu-firewall-config.png](ubuntu-firewall-config.png) — `ufw` limitado a `192.168.0.0/24`
- [wireshark-filtro-puerto-8000.png](wireshark-filtro-puerto-8000.png) — captura de práctica local (demo previa, no evidencia oficial contra el servidor real)

Limitación documentada: la regla de detección también puede marcar tráfico legítimo repetitivo (falsos positivos), no es concluyente por sí sola.
