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
