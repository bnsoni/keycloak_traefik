version: "3.7"

networks: 
  traefik:
    external: true

services:
  keycloak_db:
    image: postgres
    environment:
      - POSTGRES_DB=keycloak
      - POSTGRES_USER=keycloak
      - POSTGRES_PASSWORD=keycloak
      - POSTGRES_ROOT_PASSWORD=keycloak
    networks:
      - traefik
    volumes:
      - ./data:/var/lib/postgresql/data
    labels:
      - "traefik.enable=false"

  keycloak:
    image: jboss/keycloak:latest
    hostname: keycloak
    environment:
      - DB_VENDOR=POSTGRES
      - DB_ADDR=keycloak_db
      - DB_DATABASE=keycloak
      - DB_PORT=5432
      - DB_USER=keycloak
      - DB_SCHEMA=public
      - DB_PASSWORD=keycloak
      - PROXY_ADDRESS_FORWARDING=true
      - KEYCLOAK_LOGLEVEL=INFO
      - KEYCLOAK_USER=keycloak_admin
      - KEYCLOAK_PASSWORD=keycloak
    networks:
      - traefik
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.keycloak.rule=Host(`keycloak.bnsoni.com`)"
      - "traefik.http.routers.keycloak.entrypoints=websecure"
      - "traefik.http.routers.keycloak.tls.certresolver=myresolver"
    depends_on:
      - keycloak_db
