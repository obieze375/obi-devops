
[[Docker]] [[Kubernetes]]  [[Index]] [[Kubernetes Troubleshooting]]

## docker-compose.yml

  

```yml

version: '3'

  

  

services:

  

  web:

  

    build: .

  

    # build from Dockerfile

  

    context: ./Path

  

    dockerfile: Dockerfile

  

    ports:

  

     - "5000:5000"

  

    volumes:

  

     - .:/code

  

  redis:

  

    image: redis

  

```

  

  

## Installing compose

  

  

```

  

    •    curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

  

    •    chmod +x /usr/local/bin/docker-compose

  

    •    ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

  

    •    docker-compose --version

  

    •    history

  

  

```

  

  

## Common commands

  

  
  
  

# Starts existing containers for a service.

  

```bash

  

docker-compose start

  

```

  

# Stops running containers without removing them.

  

```

docker-compose stop

  

```

  

# Pauses running containers of a service.

  

```

docker-compose pause

  

```

  

# Unpauses paused containers of a service.

  

```

docker-compose unpause

  

```

  

# Lists containers.

  

```

docker-compose ps

  

```

  

# Builds, (re)creates, starts, and attaches to containers for a service.

  

```

docker-compose up

  

```

  

# Stops containers and removes containers, networks, volumes, and images created by up.

  

```

docker-compose down

  

```

  

# Config file reference

  

  

## Building

  

  

```yml

  

web:

  

  # build from Dockerfile

  

  build: .

  

  # build from custom Dockerfile

  

  build:

  

    context: ./dir

  

    dockerfile: Dockerfile.dev

  

  # build from image

  

  image: ubuntu

  

  image: ubuntu:14.04

  

  image: tutum/influxdb

  

  image: example-registry:4000/postgresql

  

  image: a4bc65fd

  

```

  

  

## Ports

  

  

```yml

  

ports:

  

  - "3000"

  

  - "8000:80"  # guest:host

  

# expose ports to linked services (not to host)

  

expose: ["3000"]

  

```

  

  

## Commands

  
  
  
  

# command to execute

  

```yml

command: bundle exec thin -p 3000

  

command: [bundle, exec, thin, -p, 3000]

```

  

# override the entrypoint

  

```yml

entrypoint: /app/start.sh

  

entrypoint: [php, -d, vendor/bin/phpunit]

  

```

  

  

## Environment variables

  

  
  
  

# environment vars

  

```yml

  

environment:

  

  RACK_ENV: development

  

environment:

  

  - RACK_ENV=development

```

  

# environment vars from file

  

```yml

  

env_file: .env

  

env_file: [.env, .development.env]

  

```

  

  

## Dependancies

  

  
  
  

# makes the `db` service available as the hostname `database`

  

# (implies depends_on)

  

```yml

  

links:

  

  - db:database

  

  - redis

```

  

# make sure `db` is alive before starting

  

```yml

  

depends_on:

  

  - db

  

```

  

  

## Other Options

  

  

# make this service extend another

  

```yml

  

extends:

  

  file: common.yml  

  

  service: webapp

  

volumes:

  

  - /var/lib/mysql

  

  - ./_data:/var/lib/mysql

  

```

  

  

## Labels

  

  

```yml

  

services:

  

  web:

  

    labels:

  

      com.example.description: "Accounting web app"

  

```

  

  

## DNS Servers

  

  

```yml

  

services:

  

  web:

  

    dns: 8.8.8.8

  

    dns:

  

      - 8.8.8.8

  

      - 8.8.4.4

  

```

  

  

## Devices

  

  

```yml

  

services:

  

  web:

  

    devices:

  

      - "/dev/ttyUSB0:/dev/ttyUSB0"

  

```

  

  

## External Links

  

  

```yml

  

services:

  

  web:

  

    external_links:

  

      - redis_1

  

      - project_db_1:mysql

  

```

  

  

## Hosts

  

  

```yml

  

services:

  

  web:

  

    extra_hosts:

  

      - "somehost:192.168.1.100"

  

```

  

  

## Networks

  

# creates a custom network called `frontend`  

  

```yml

  
  
  

networks:

  

  frontend:

  

```

  

  

## External Networks

  

# join a preexisting network

  

```yml

  

networks:

  

  default:

  

    external:

  

      name: frontend

  

```

## Connect to a container

  

  

Use the docker exec Command to Connect to a Running Container

  

  

The docker exec is used to connect to a container that is already running. You can use the docker exec command to get a bash shell in the running container or run any command directly inside the container.

  

  

Get a Bash Shell in the Container

  

  

The basic syntax to get a bash shell in the running container is shown below:

  

  

```bash

docker exec -it container-name /bin/bash

  

```

  

  

Or

  

  

```

  

docker container exec -it container-name /bin/bash

  

```

  

First, run the docker ps command to get the name of the existing container:

  

  

```

  

docker ps

  

```

  

  

You should get all running containers in the following output:

  

  

```

  

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES

  

51e83428cee5        httpd               "httpd-foreground"       5 seconds ago       Up 3 seconds        80/tcp              apache-container

  

  

5f8c42bfd237        nginx               "/docker-entrypoint.…"   29 seconds ago      Up 27 seconds       80/tcp              nginx-container

  

```

  
Next, choose the name of the running container from the above list and run the following command to get a bash shell in the container.

```
docker container exec -it nginx-container /bin/bash                                            
```