# Preguntas de análisis — Laboratorio 3 (sección 12 del PDF)

**¿Qué pudo observar el Red Team sin explotar ninguna vulnerabilidad?**
Pudimos observar la versión del servidor (nginx 1.28.3), el sistema operativo base (Ubuntu vía nmap CPE), y el contenido completo del sitio sin pedir credenciales. Al copiar deliberadamente la carpeta `.git` al docroot para simular el riesgo, también se pudo ver el contenido de un archivo de configuración interno. Todo esto con herramientas de reconocimiento pasivo (nmap, curl, ZAP en modo Manual Explore), sin lanzar ningún exploit.

**¿Qué pruebas de red no aparecieron en access.log y por qué?**
El escaneo TCP inicial de puertos (SYN scan) de Nmap no deja rastro en `access.log`, porque ese log solo registra peticiones HTTP que llegan a la aplicación. El escaneo de puertos ocurre a nivel de red/transporte, antes de que exista una conexión HTTP completa. Solo quedaron registradas las peticiones de la fase `-sV` (detección de servicio), que sí completan el handshake HTTP.

**¿Qué control aplicado reduce exposición, pero no resuelve el riesgo de HTTP?**
`server_tokens off` y los headers de seguridad (`X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`) disminuyen la información que un atacante puede recolectar y mitigan ataques específicos (clickjacking, MIME sniffing). Pero el tráfico sigue viajando sin cifrar: cualquiera con acceso a la red todavía puede capturar el contenido completo con un PCAP. El riesgo de fondo (confidencialidad e integridad en tránsito) solo se resuelve con HTTPS/TLS.

**¿Qué datos necesitaría Blue Team para distinguir curl legítimo de una actividad sospechosa?**
Con el `access.log` actual solo se tiene IP, timestamp, método, ruta, status y User-Agent — insuficiente para diferenciar un curl de un desarrollador legítimo del malicioso, ya que el User-Agent se puede falsificar fácilmente. Se necesitaría identidad autenticada (usuario/token), un sistema de reputación de IP, o correlación con el contexto.

**¿Qué amenaza STRIDE debe priorizarse en el Laboratorio 4?**
Spoofing e Information Disclosure, en conjunto: sin autenticación cualquiera puede hacerse pasar por un operador autorizado, y sin cifrado las credenciales que se introduzcan viajarían en texto plano. Ambas se resuelven juntas con TLS + identidad, y es la base para que luego se pueda hablar de Repudiation real.

**¿Qué conclusión propuesta por la IA no pudo comprobarse directamente?**
Claude interpretó que las rutas que probó Nmap (`/HNAP1`, `/evox/about`, `/sdk`) eran "firmas típicas de escaneo de vulnerabilidades", pero eso no se verificó contra qué script NSE de Nmap generó cada petición — es una interpretación plausible sin confirmar con evidencia propia.
