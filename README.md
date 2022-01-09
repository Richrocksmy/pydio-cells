# Pydio Cells Private Cloud

This is a private cloud implementation of Pydio Cells using Collabora Office's separate document server

# Pre-requisites

Three DNS A records (monitor.ovcloud.io, blog.ovcloud.io, db-admin.ovcloud.io) pointing to the server that the containers will be running on.

# Installation Steps

1. Run ```install-docker.sh``` to install Docker and Docker Compose
2. Run a Traefik container with the following command:

```java
    docker run -d \
      -v /var/run/docker.sock:/var/run/docker.sock \
      -v $PWD/traefik.toml:/traefik.toml \
      -v $PWD/traefik_dynamic.toml:/traefik_dynamic.toml \
      -v $PWD/acme.json:/acme.json \
      -p 80:80 \
      -p 443:443 \
      --network web \
      --name traefik \
      traefik:v2.2
```

3. Export the environment variables by executing ```source .env```
4. Start Wordpress and the database - ```docker-compose up -d```

To stop the services run - ```docker-compose down```

Stop the Traefik container - Use ```docker ps``` to find the container id, then run ```docker stop {container_id}```



