https://docs.docker.com/compose/gettingstarted/

Docker compose is a way to create "cookbooks" for the setting up (and taking down) one or more docker containers. it is written in yaml. 
This makes sure that the container is set up, and pulled down again in the same way every time.
We can also pin a container to a specific version with this.

example:
```bash
---
version: "3"
services:
  yacht:
    container_name: yacht
    restart: unless-stopped
    ports:
      - 8000:8000
    volumes:
      - yacht:/config
      - /var/run/docker.sock:/var/run/docker.sock
    image: selfhostedpro/yacht

volumes:
  yacht:
```

Each docker compose project should be housed in its own folder.

Running a docker-compose file is done with the command "docker-compose up". I added -d to detach from the container and leave it running. Else it will take over the terminal
```bash
root@dockerhost:~/dockercompose/yacht# docker-compose up -d
Starting yacht ... done
root@dockerhost:~/dockercompose/yacht# portainer ps
Could not find command-not-found database. Run 'sudo apt update' to populate it.
portainer: command not found
root@dockerhost:~/dockercompose/yacht# docker ps
CONTAINER ID   IMAGE                 COMMAND   CREATED        STATUS          PORTS                                       NAMES
b0a3022c1921   selfhostedpro/yacht   "/init"   20 hours ago   Up 15 seconds   0.0.0.0:8000->8000/tcp, :::8000->8000/tcp   yacht
```

To stop a container and tear it down, we use the "docker-compose down" command.
This stops the container, and then removes it again.
```bash
root@dockerhost:~/dockercompose/yacht# docker-compose down
Stopping yacht ... done
Removing yacht ... done
Removing network yacht_default
root@dockerhost:~/dockercompose/yacht# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```