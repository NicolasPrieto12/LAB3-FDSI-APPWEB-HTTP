# nginx/

- `site.conf` — virtual host del Paso 4, para copiar tal cual a `/etc/nginx/sites-available/muvautomation` en el Ubuntu Server.
- `hardening.conf` — headers de seguridad del Paso 15, se agrega DESPUES del primer retest (no antes, porque el lab pide medir el "antes" sin hardening).

Firewall (Paso 5), correr en el Ubuntu una vez Nginx este arriba:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow from "$LAB_CIDR" to any port 80 proto tcp
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status numbered
```
