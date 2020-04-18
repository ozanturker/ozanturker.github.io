# Keycloak with Postgres

In order to run Keycloak with Postgres some configurations are needed. Required configurations are shown in Requirements section. Also runtime configurations are shown in Docker Compose yaml file.

## Modules

- Keycloak 9.0.0
- Posgres 10.4

## Requirements

- Docker compose

```
sudo apt install docker-compose 
```

- Docker network

Docker compose needed test network. 'test' is name of network. Docker network creation shown below.

```
docker network create test
```

## Docker Compose

```yaml
version: '3.3'

services:
  postgres:
    container_name: postgres_container
    image: postgres:10.4
    environment:
      POSTGRES_USER: postgres #postgres user
      POSTGRES_PASSWORD: "123456" # user  password
      POSTGRES_DB: keycloak # database name
    volumes:
    # local path : docker path
      - /var/lib/backup/keycloak_pg_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    restart: unless-stopped
    networks:
      - test
  keycloak:
    container_name: keycloak_container
    image: jboss/keycloak:9.0.0
    environment:
      KEYCLOAK_USER: admin # keycloak login user
      KEYCLOAK_PASSWORD: 123456 # keycloak login user password
      DB_ADDR: "postgres" # database adress (postgres service name)
      DB_PORT: "5432"
      DB_DATABASE: "keycloak" # postgres database name
      DB_USER: postgres # postgres database user
      DB_PASSWORD : "123456"  # postgres database user password
      DB_VENDOR: postgres
    ports:
      - "8080:8080"
    restart: unless-stopped
    depends_on:
      - postgres
    networks:
      - test
networks:
  test:
    external: true
```

## Docker Compose 101

### How to run ?

```
docker-compose up -d
```

> -d : if you want to run the container in the background in a “detached” mode

### How to remove ?

```
docker-compose down
```

### How to start ?

```
docker-compose start
```

### How to stop ?

```
docker-compose stop
```

## Related Sources

- https://hub.docker.com/r/jboss/keycloak/
- https://docs.docker.com/compose/
- https://hub.docker.com/_/postgres