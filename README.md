# Docker-Compose cheat sheet
A cheet sheat to create you docker-compose.yml

#cheatsheet #docker #docker-compose

`version: '3.8'`

## Build the image

```yaml
web:
  # build from Dockerfile
  build: . # local path or repo
  # alternative to use a folder with more than one Dockerfile:
  build:
    context: ./dir #path to Dockerfile
    dockerfile: Dockerfile.dev #name of the file
    
  # build from image
  image: ubuntu
  image: ubuntu:14.04
```

## Expose port

```yaml
ports:
  - "3000"
  - "8000:80"  # guest:host
# expose ports to linked services (not to host)
expose: ["3000"]
```

### Commands and Entrypoint

```yaml
# command to execute
command: bundle exec thin -p 3000
command: [bundle, exec, thin, -p, 3000]

# override the entrypoint
entrypoint: /app/start.sh
entrypoint: [php, -d, vendor/bin/phpunit]
```

### Environment variables

```yaml
# environment vars
environment:
  RACK_ENV: development
environment:
  - RACK_ENV=development

# environment vars from file
env_file: .env
env_file: [.env, .development.env]
```

## Health check

```yaml
# declare service healthy when `test` command succeed
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost"]
  interval: 1m30s
  timeout: 10s
  retries: 3
  start_period: 40s
```

### Dependencies

```yaml
# make sure `db` is alive before starting
depends_on:
  - db


# make sure `db` is healty before starting
# and db-init completed without failure
depends_on:
db:
  condition: service_healthy
db-init:
  condition: service_completed_successfully

```

## Volumes

```yaml
volumes:
  - /var/lib/mysql # anonimous volume
  - ./host_path:/docker_container_path # bind mount
```

## Restart

```yaml
# automatically restart container
restart: unless-stopped
# always, on-failure, no (default)
```

## User

```yaml
# specifying user
user: root
```

```yaml
# specifying both user and group with ids
user: 0:0
```
## Commands
``` shell
# Starts existing containers.
docker-compose start

# Stops running containers.
docker-compose stop

# Pauses running containers.
docker-compose pause

# Unpauses paused.
docker-compose unpause

# Lists containers.
docker-compose ps

# Builds, (re)creates, starts, and attaches to containers for a service.
docker-compose up

# Stops and removes containers.
docker-compose down
```


Based on https://devhints.io/docker-compose
