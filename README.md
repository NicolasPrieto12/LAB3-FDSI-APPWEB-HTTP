<p align="center">
  <img src="assets/logo.svg" width="140" alt="Network Automation Hub logo">
</p>

<h1 align="center">Network Automation Hub</h1>
<p align="center">Laboratorio 3 — Aplicaciones Web / HTTP</p>

## Que es esto

Vamos a construir un prototipo para consultar un inventario ficticio de dispositivos de red (firewalls, routers, switches) y preparar la ejecucion controlada de scripts previamente aprobados sobre ellos. La idea es que quede evidencia clara de quien ejecuto que accion y con que resultado, en vez de que cada quien le meta mano a los equipos por su cuenta.

Durante este laboratorio solo se permiten operaciones de lectura y simulaciones. No se conecta a equipos reales ni se hacen cambios reales todavia.

## En que vamos

Por ahora solo tenemos lista la referencia de la API que vamos a implementar: [`network-automation-hub/index.html`](network-automation-hub/index.html). Es una pagina estatica (sin backend, sin nada que instalar) que documenta los endpoints planeados:

- Inventario de dispositivos
- Scripts aprobados y su validacion
- Ejecucion controlada (simulada), rollback y evidencia/auditoria

Se abre directo en el navegador, no necesita servidor.

Lo que sigue es implementar esos endpoints de verdad.

## Captura de trafico HTTP (Wireshark)

Como parte del lab tocaba comprobar que el trafico de la pagina de verdad viaja como HTTP plano. Para eso:

1. Servimos la pagina con un servidor HTTP local (no abrirla como `file://`, porque ahi el navegador no genera trafico de red):
   ```bash
   cd network-automation-hub
   python -m http.server 8000
   ```
2. En Wireshark, capturando sobre el **Adapter for loopback traffic capture** (el trafico a `localhost` no sale por la tarjeta de red normal), aplicamos el filtro:
   ```
   tcp.port == 8000
   ```
3. Con la captura corriendo, se abrio `http://localhost:8000/` en Chrome.

### Lo que se vio filtrado por el puerto 8000

| # paquete | Protocolo | Info |
|---|---|---|
| 1849 | HTTP | `GET / HTTP/1.1` |
| 1853 | HTTP | `HTTP/1.0 200 OK (text/html)` |
| 1863 | HTTP | `GET /favicon.ico HTTP/1.1` |
| 1867 | HTTP | `HTTP/1.0 404 File not found (text/plain)` |

El 404 del `favicon.ico` es esperado, la pagina no tiene uno — el navegador lo pide solo por costumbre.

### HTTP stream completo (Follow → HTTP Stream del paquete GET /)

Request de Chrome y respuesta del servidor, capturados en texto plano porque todo corrio por HTTP sin cifrar:

```
GET / HTTP/1.1
Host: localhost:8000
Connection: keep-alive
sec-ch-ua: "Chromium";v="152", "Not?A_Brand";v="24", "Google Chrome";v="152"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/152.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br, zstd
Accept-Language: es-US,es-419;q=0.9,es;q=0.8

HTTP/1.0 200 OK
Server: SimpleHTTP/0.6 Python/3.14.6
Date: Wed, 02 Sep 2026 23:36:22 GMT
Content-type: text/html
Content-Length: 14097
Last-Modified: Wed, 02 Sep 2026 23:17:24 GMT

<!DOCTYPE html>
...
```

**Que muestra esto:** que la pagina viaja como HTTP sin cifrar (por eso Wireshark puede leer el texto tal cual), que el navegador manda sus headers normales de una peticion de navegacion (`Sec-Fetch-*`, `User-Agent`, `Accept`), y que el servidor responde `200 OK` con el HTML completo (`Content-Length: 14097`, coincide con el tamano real de `index.html`).
