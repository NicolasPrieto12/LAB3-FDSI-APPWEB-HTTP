# Reflexiones individuales: Laboratorio 3

## Mabel

Este laboratorio me enseñó que la parte técnica de seguridad no es lo más difícil. Lo más difícil fue simplemente lograr que las máquinas se vieran entre sí. Pasamos horas mirando cómo hacer que se vieran las máquinas. Redes bridged que no agarraban IP un teclado que se dañó solo y un NAT que aislaba mi Ubuntu del resto de la red. Al principio pensé que estábamos muy atrasados pero entendí que el profe no estaba preguntando si teníamos algo en la nube. Estaba preguntando si teníamos una instancia Linux real corriendo. Algo que nosotros mismos podíamos montar en una VM local sin necesidad de un servidor pagado.

Lo que más me marcó fue ver en vivo con Wireshark que todo el tráfico HTTP viaja en texto plano. Pude leer el request y la respuesta completa como si fuera un archivo de texto normal. Eso hizo que "Information Disclosure" dejara de ser una definición abstracta y se volviera algo que vi con mis propios ojos. Lo mismo con Nmap. Un solo comando reveló la versión exacta del servidor sin necesidad de hackear nada.

También aprendí que este laboratorio deja cosas rotas a propósito. Sin HTTPS y sin autenticación real para resolverlas en el Laboratorio 4. Eso me ayudó a entender que la seguridad se construye por capas y no de una sola vez.

Trabajar en equipo a distancia cada uno en su red también fue un reto. Coordinar quién hacía qué y cuándo terminó siendo casi tan importante como el contenido técnico.

## Nicolás

Antes de hacer este laboratorio sabía que HTTP es inseguro, pero con la hechura de este cambió lo que pensaba con del riesgo. Lo más revelador fue notar cuánta información se filtra sin que nadie "hackee" nada: el simple header Server: nginx/1.28.3 (Ubuntu) le dice a cualquiera qué versión exacta está corriendo, y eso ya es una pista para buscar vulnerabilidades. También me sorprendió ver en “access.log” que un escaneo de Nmap deja su propia firma, es decir cuatro peticiones a rutas absurdas (‘/HNAP1’, ‘/evox/about’) en el mismo segundo exacto, con un User-Agent que dice "Nmap Scripting Engine". Sin saber nada de seguridad, cualquiera que revise ese log se da cuenta de que algo no fue tráfico humano normal.

Lo que más me impacto fue el hardening: con apenas seis líneas de configuración en Nginx (‘server_tokens off’, algunos headers, una regla de ‘deny’) corregimos dos de los cinco riesgos que había detectado ZAP, sin tocar nada de la lógica de la aplicación. Con lo antes dicho me dejó claro que buena parte de la seguridad básica es configuración, no código.

La conexión con el Laboratorio 4, podemos asumir que cualquiera puede leer el tráfico y entrar sin identificarse. R1 (HTTP sin cifrar) y R3 (sin autenticación) quedaron explícitamente, porque son justo lo que viene, cifrar con TLS para que ni el contenido ni las credenciales viajen en claro, y agregar identidad para que el ‘access.log’ deje de mostrar solo una IP anónima y empiece a mostrar quién hizo qué.

