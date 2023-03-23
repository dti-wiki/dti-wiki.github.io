# Caddy Container

Using a Caddy Server as a reverse proxy to route traffic to adminer.

## Caddyfile

Below is the Caddyfile v2

```
your-name.domain.com

encode gzip
reverse_proxy /sql adminer:8080
tls your-email@domain.com

```

## Docker Compose

Below is the `docker-compose.yml` file for Caddy Server.

```
version: "3.2"

services:
  sql1:
    container_name: sql1
    image: mcr.microsoft.com/mssql/server:2022-latest
    tty: true
    ports:
      - "135:135"
      - "1431:1431"
      - "1433:1433"
      - "1434:1434"
    restart: always
    environment:
      MSSQL_SA_PASSWORD: fake1234567890xyz
      SA_PASSWORD: fake1234567890xyz
      ACCEPT_EULA: "Y"
    volumes:
      - /home/user/service/dumps:/dumps
      - /home/user/service/db/data:/var/opt/mssql/data

  caddy:
    image: caddy:2.6.2-alpine
    container_name: caddy
    restart: always
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - /home/user/service/caddy/Caddyfile:/etc/caddy/Caddyfile
      
  adminer:
    container_name: adminer
    image: adminer:latest
    ports:
      - "8080:8080"
    links:
      - sql1
    depends_on:
      - sql1
    restart: always
    ```
