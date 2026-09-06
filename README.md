# CV

Repository for my CV website : http://jordan.penard.fr

QR code generated with : https://www.qr-code-generator.com/solutions/vcard-qr-code/
Color : #910691

## Deployment (Apache in Docker)

    # on the server, from this repo:
    docker compose up -d

Apache serves `./html` on host port 8081. A reverse proxy maps
jordan.penard.fr to `http://localhost:8081`.

Config notes:
- `allow-htaccess.conf` is mounted into `/etc/apache2/conf-enabled/` so
  `.htaccess` overrides are honoured for `/var/www/html`.
- `headers.load` is mounted into `/etc/apache2/mods-enabled/` because the
  `ubuntu/apache2` rock does not enable `mod_headers` by default. This makes
  the `Cache-Control` rules in `html/.htaccess` take effect.

Smoke test after `docker compose up -d`:

    curl -I http://localhost:8081/              # expect Cache-Control: no-cache
    curl -o /dev/null -w "%{http_code}\n" http://localhost:8081/style.css?v=2
