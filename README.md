# swarmpit-traefik-pgadmin4-postgres13
```
version: '3.3'
services:
  pgadmin:
    image: dpage/pgadmin4:4.6
    labels:
      traefik.http.routers.pgadmin-http.service: pgadmin
      traefik.http.middlewares.pgadmin-https-redirect.redirectscheme.scheme: https
      traefik.http.routers.pgadmin-https.service: pgadmin
      traefik.http.routers.pgadmin-https.tls: 'true'
      traefik.http.routers.pgadmin-https.rule: Host(`pgadmin4.odooerp.online`)
      traefik.http.routers.pgadmin-http.entrypoints: http
      traefik.http.routers.pgadmin-https.tls.certresolver: letsencrypt
      swarmpit.service.deployment.autoredeploy: 'true'
      traefik.http.routers.pgadmin-http.rule: Host(`pgadmin4.odooerp.online`)
      traefik.http.services.pgadmin.loadbalancer.server.port: '5050'
      traefik.http.routers.pgadmin-http.middlewares: pgadmin-https-redirect
      traefik.http.routers.pgadmin-https.entrypoints: https
      traefik.enable: 'true'
    environment:
      PGADMIN_DEFAULT_EMAIL: mfalconsoft@gmail.com
      PGADMIN_DEFAULT_PASSWORD: 123
      PGADMIN_LISTEN_PORT: '80'
    ports:
     - 5050:80
    volumes:
     - app-postgres:/var/lib/postgresql/data/pgdata
    networks:
     - app-postgree
     - traefik
    logging:
      driver: json-file
  postgres13:
    image: postgres:13
    environment:
      PGDATA: /var/lib/postgresql/data/pgdata
      POSTGRES_DB: postgres
      POSTGRES_PASSWORD: odoo
      POSTGRES_USER: odoo
    volumes:
     - app-postgres:/var/lib/postgresql/data/pgdata
    networks:
     - app-postgree
    logging:
      driver: json-file
networks:
  app-postgree:
    driver: overlay
  traefik:
    external: true
volumes:
  app-postgres:
    driver: local
 ```
