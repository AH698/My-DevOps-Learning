# Docker

Docker is a platform used to package applications
and their dependencies into containers.

This allows applications to run consistently
across different environments.

## Docker Images and Containers

A Docker image is a blueprint used to create
containers.

It contains the application, dependencies and
configuration needed for the application to run.

A container is a running instance of an image.

```
docker images
docker ps
docker ps -a
```

`docker images` lists Docker images.

`docker ps` shows running containers.

`docker ps -a` shows all containers, including
stopped ones.

## Dockerfile

A Dockerfile contains the instructions Docker
follows to build an image.

```
FROM python:3.8-slim
WORKDIR /app
COPY . .
RUN pip install flask redis
EXPOSE 5005
CMD ["python", "app.py"]
```

`FROM` sets the base image.

`WORKDIR` sets the working directory inside
the container.

`COPY` copies files into the image.

`RUN` executes commands while building the image.

`EXPOSE` shows the port the application uses.

`CMD` defines what runs when the container starts.

An image can then be built using:

```
docker build -t my-app .
```

## Docker Compose

Docker Compose is used to define and manage
applications that use multiple containers.

The services are defined inside a
`docker-compose.yml` file.

For example, Flask, Redis and NGINX can all run
as separate services within the same application.

```
docker compose up -d
docker compose up -d --build
docker compose down
```

Docker Compose also creates a network for the
services.

This allows containers to communicate using
their service names.

For example, Flask can connect to Redis using
`redis` as the hostname instead of `localhost`.

## Docker Networking

Docker networks allow containers to communicate
with each other.

When using Docker Compose, services are
automatically placed on the same network.

Service names can then be used as hostnames.

For example:

```
redis.Redis(host='redis', port=6379)
```

`redis` identifies the Redis service.

`6379` is the port Redis listens on.

`localhost` would refer to the Flask container
itself, not the Redis container.

## Ports

Port mappings allow applications inside
containers to be accessed from the host machine.

```
ports:
  - "5005:5005"
```

The format is:

```
HOST_PORT:CONTAINER_PORT
```

The first port is on the host machine.

The second port is the port being used inside
the container.

`expose` can be used when a service only needs
to be available internally to other containers.

## Volumes

Docker volumes provide persistent storage.

Containers can be deleted and recreated, while
data stored inside a volume can remain.

```
volumes:
  - redis-data:/data
```

`redis-data` is the named Docker volume.

`/data` is the directory inside the Redis
container where Redis writes its persistent data.

This allows Redis data to survive when the
container is recreated.

## Environment Variables

Environment variables allow configuration values
to change without changing the application code.

```
environment:
  - REDIS_HOST=redis
  - REDIS_PORT=6379
```

The application can read these values using:

```
redis_host = os.getenv('REDIS_HOST', 'redis')
redis_port = int(os.getenv('REDIS_PORT', 6379))
```

This makes the application more flexible because
settings can be changed through the environment
instead of being hardcoded into the application.

## NGINX

NGINX can be used as a reverse proxy and
load balancer.

As a reverse proxy, NGINX receives requests
and forwards them to the application.

As a load balancer, NGINX can distribute requests
between multiple instances of an application.

```
upstream flask_app {
    server web:5005;
}

server {
    listen 5005;

    location / {
        proxy_pass http://flask_app;
    }
}
```

`upstream` defines the backend application.

`listen` defines the port NGINX listens on.

`proxy_pass` forwards requests to the backend.

## Scaling

Scaling means running multiple instances of
a service to provide more capacity.

Docker Compose can scale a service using:

```
docker compose up --scale web=3
```

This creates three instances of the `web` service.

NGINX can then distribute incoming requests
between the application instances.

## Useful Docker Commands

```
docker images
docker ps
docker ps -a
docker build -t image-name .
docker logs container-name
docker compose up -d
docker compose up -d --build
docker compose down
docker compose ps
docker compose logs
```

These commands cover viewing images and containers,
building images, checking logs and managing
multi-container applications with Docker Compose.