# reports/zap-passive: OWASP ZAP, Manual Explore (completo)

Escaneo pasivo contra `http://192.168.0.6` (sin Active Scan), 23/09/2026.

**Resultado: 5 alertas pasivas, 0 High, 3 Medium, 2 Low**

| Alerta | Riesgo |
|---|---|
| Content Security Policy (CSP) Header Not Set | Medium |
| Missing Anti-clickjacking Header | Medium |
| Sub Resource Integrity Attribute Missing | Medium |
| Server Leaks Version Information via "Server" HTTP Response Header Field | Low, relacionado con R2 |
| X-Content-Type-Options Header Missing | Low |

Ver [zap-alerts-summary.png](zap-alerts-summary.png) y [zap-report-risk-table.png](zap-report-risk-table.png).

Nota: falta subir el `Report.html` exportado completo de ZAP. Segun las capturas quedo guardado en el escritorio de Kali (`~/Desktop/Report.html`), hay que pedirselo a Nicolas para tenerlo integro en el repo.
