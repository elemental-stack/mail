# Modern Self-hosted Mail Stack

- https://github.com/kereis/traefik-certs-dumper


## FIXME: Code sample

```yml
  proxy: # ...
  mailhost:
    image: crccheck/hello-world
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.mailhost.rule=Host(`mail.example.com`)"
      - "traefik.http.routers.mailhost.entrypoints=websecure"
      - "traefik.http.routers.mailhost.service=mailhost"
      - "traefik.http.routers.mailhost.tls.certresolver=myresolver"
      - "traefik.http.routers.mailhost-plain.entrypoints=web"
      - "traefik.http.routers.mailhost-plain.rule=Host(`mail.example.com`)"
      - "traefik.http.routers.mailhost-plain.middlewares=redirect-https"
      - "traefik.http.services.mailhost.loadbalancer.server.port=8000"
    networks:
      - my_network
  haraka:
    build: .
    ports:
      - "25:25"
      - "587:587"
    volumes:
      - ./haraka:/haraka
      - ../proxy/certs:/etc/letsencrypt/live:ro
    environment:
      - VIRTUAL_HOST=${MAIL_VIRTUAL_HOST}
      - LETSENCRYPT_HOST=${MAIL_VIRTUAL_HOST}
    networks:
      - my_network
networks:
  my_network:
    external: true

```
