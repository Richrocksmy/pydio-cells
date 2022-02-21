# Pydio Cells Private Cloud

This is a private cloud implementation of Pydio Cells using Collabora Office's separate document server

# Pre-requisites

Three DNS A records (monitor.ovcloud.io, blog.ovcloud.io, db-admin.ovcloud.io) pointing to the server that the containers will be running on.

# Installation Steps

1. Run ```install-docker.sh``` to install Docker and Docker Compose
2. Create the external docker network using - ```docker network create web```
3. Run a Traefik container with the following command:

```shell
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

# Notes

Used [this](https://www.digitalocean.com/community/tutorials/how-to-use-traefik-v2-as-a-reverse-proxy-for-docker-containers-on-ubuntu-20-04) article for reference.

[Useful info about Libreoffice docker](https://github.com/smehrbrodt/nextcloud-libreoffice-online/blob/master/libreoffice-online/.env.sample)

# Todo

- ```CELLS_NO_TLS=1``` TLS is cells has been disabled, is this OK because ssl termination is handled by Traefik?
- Create docker networks automatically
- Add Traefik to docker compose
- Connect from phone / other devices (potential problems with firewall and gRPC ports)
- Use postgres instead of MySql
- Remove phpMyAdmin
- Tidy up security / passwords
- Lock docker containers at specific versions
- Use environment values in the same way across docker compose

Current state:

Need to figure out how to connect to Collabora - can connect to office.ovcloud.io but get 502 when opening document
