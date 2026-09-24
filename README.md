# Docker CLI Cheat Sheet & DevOps

A practical Docker quick-reference, deployments, troubleshooting, Docker Compose, images, volumes, networks, cleanup, and production operations.

> **Copy buttons:** Every command is placed in a fenced code block. GitHub, VS Code, Obsidian, ChatGPT, and many modern Markdown viewers automatically show a **Copy** button for fenced code blocks.

---

## Index

1. [Docker Service](#1-docker-service)
2. [Help & Reference](#2-help--reference)
3. [Containers](#3-containers)
4. [Container Lifecycle](#4-container-lifecycle)
5. [Execute Commands Inside Containers](#5-execute-commands-inside-containers)
6. [Container Logs](#6-container-logs)
7. [Container Inspection](#7-container-inspection)
8. [Resource Usage](#8-resource-usage)
9. [Copy Files](#9-copy-files)
10. [Images](#10-images)
11. [Build Images](#11-build-images)
12. [Tag Images](#12-tag-images)
13. [Registry & Docker Hub](#13-registry--docker-hub)
14. [Save & Load Images](#14-save--load-images)
15. [Volumes](#15-volumes)
16. [Networks](#16-networks)
17. [Docker Disk Usage](#17-docker-disk-usage)
18. [Docker Cleanup](#18-docker-cleanup)
19. [Docker Compose: Start](#19-docker-compose-start)
20. [Docker Compose: Status](#20-docker-compose-status)
21. [Docker Compose: Stop / Start / Restart](#21-docker-compose-stop--start--restart)
22. [Docker Compose: Down](#22-docker-compose-down)
23. [Docker Compose: Logs](#23-docker-compose-logs)
24. [Docker Compose: Exec / Shell](#24-docker-compose-exec--shell)
25. [Docker Compose: One-off Commands](#25-docker-compose-one-off-commands)
26. [Docker Compose: Build](#26-docker-compose-build)
27. [Docker Compose: Validate Config](#27-docker-compose-validate-config)
28. [Docker Compose: Multiple Files](#28-docker-compose-multiple-files)
29. [Buildx](#29-buildx)
30. [Ports](#30-ports)
31. [Docker Events](#31-docker-events)
32. [Filters](#32-filters)
33. [Formatted Output](#33-formatted-output)
34. [Restart Policies](#34-restart-policies)
35. [Resource Limits](#35-resource-limits)
36. [Docker Context](#36-docker-context)
37. [Docker Socket & Permissions](#37-docker-socket--permissions)
38. [Docker Daemon Logs](#38-docker-daemon-logs)
39. [Docker Storage Paths](#39-docker-storage-paths)
40. [Find Large Docker Logs](#40-find-large-docker-logs)
41. [Production Troubleshooting](#41-production-troubleshooting)
42. [Most-used Commands](#42-most-used-commands)
43. [Deployment Quick Flow](#43-deployment-quick-flow)
44. [Official Docker References](#44-official-docker-references)

---

# 1. Docker Service

Check Docker version:

```bash
docker --version
```

```bash
docker version
```

Show Docker system information:

```bash
docker info
```

Check Docker service:

```bash
sudo systemctl status docker
```

Start Docker:

```bash
sudo systemctl start docker
```

Stop Docker:

```bash
sudo systemctl stop docker
```

Restart Docker:

```bash
sudo systemctl restart docker
```

Enable Docker on boot:

```bash
sudo systemctl enable docker
```

Enable Docker and containerd on boot:

```bash
sudo systemctl enable docker.service
```

```bash
sudo systemctl enable containerd.service
```

Disable Docker auto-start:

```bash
sudo systemctl disable docker.service
```

```bash
sudo systemctl disable containerd.service
```

---

# 2. Help & Reference

Docker help:

```bash
docker --help
```

Container commands help:

```bash
docker container --help
```

Image commands help:

```bash
docker image --help
```

Volume commands help:

```bash
docker volume --help
```

Network commands help:

```bash
docker network --help
```

Compose help:

```bash
docker compose --help
```

Run command help:

```bash
docker run --help
```

Build command help:

```bash
docker build --help
```

Exec command help:

```bash
docker exec --help
```

---

# 3. Containers

List running containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

Show only running container IDs:

```bash
docker ps -q
```

Show all container IDs:

```bash
docker ps -aq
```

Show latest container:

```bash
docker ps -l
```

Show container sizes:

```bash
docker ps -s
```

Run an image:

```bash
docker run IMAGE
```

Example:

```bash
docker run nginx
```

Run in background:

```bash
docker run -d nginx
```

Run with a custom name:

```bash
docker run -d --name nginx nginx
```

Map a port:

```bash
docker run -d --name nginx -p 8080:80 nginx
```

Interactive shell:

```bash
docker run -it ubuntu bash
```

Delete container automatically after exit:

```bash
docker run --rm ubuntu
```

Set environment variable:

```bash
docker run -e APP_ENV=production IMAGE
```

Use an environment file:

```bash
docker run --env-file .env IMAGE
```

Bind-mount a directory:

```bash
docker run -v /host/path:/container/path IMAGE
```

Use named volume:

```bash
docker run -v mysql_data:/var/lib/mysql mysql
```

Restart automatically unless manually stopped:

```bash
docker run --restart=unless-stopped IMAGE
```

CPU limit:

```bash
docker run --cpus="2" IMAGE
```

Memory limit:

```bash
docker run --memory="1g" IMAGE
```

---

# 4. Container Lifecycle

Start container:

```bash
docker start CONTAINER
```

Stop container:

```bash
docker stop CONTAINER
```

Restart container:

```bash
docker restart CONTAINER
```

Pause container:

```bash
docker pause CONTAINER
```

Unpause container:

```bash
docker unpause CONTAINER
```

Kill container immediately:

```bash
docker kill CONTAINER
```

Remove container:

```bash
docker rm CONTAINER
```

Force remove running container:

```bash
docker rm -f CONTAINER
```

Remove multiple containers:

```bash
docker rm container1 container2
```

Remove stopped containers:

```bash
docker container prune
```

Start multiple containers:

```bash
docker start container1 container2
```

Stop all running containers:

```bash
docker stop $(docker ps -q)
```

Remove all exited containers:

```bash
docker rm $(docker ps -aq -f status=exited)
```

---

# 5. Execute Commands Inside Containers

Open Bash:

```bash
docker exec -it CONTAINER bash
```

Open `sh` when Bash is unavailable:

```bash
docker exec -it CONTAINER sh
```

Run PHP version check:

```bash
docker exec CONTAINER php -v
```

List files:

```bash
docker exec CONTAINER ls -la
```

Show environment variables:

```bash
docker exec CONTAINER env
```

Run as root:

```bash
docker exec -u root -it CONTAINER bash
```

Run from a specific working directory:

```bash
docker exec -w /var/www/html CONTAINER php artisan migrate
```

Laravel migration example:

```bash
docker exec -it laravel-app php artisan migrate
```

---

# 6. Container Logs

Show logs:

```bash
docker logs CONTAINER
```

Follow logs:

```bash
docker logs -f CONTAINER
```

Last 100 lines:

```bash
docker logs --tail 100 CONTAINER
```

Follow last 100 lines:

```bash
docker logs -f --tail 100 CONTAINER
```

Logs from last 10 minutes:

```bash
docker logs --since 10m CONTAINER
```

Add timestamps:

```bash
docker logs -t CONTAINER
```

Common troubleshooting command:

```bash
docker logs -f --tail 200 app
```

---

# 7. Container Inspection

Inspect container:

```bash
docker inspect CONTAINER
```

Inspect image:

```bash
docker inspect IMAGE
```

Inspect network:

```bash
docker inspect NETWORK
```

Inspect volume:

```bash
docker inspect VOLUME
```

Get container IP:

```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' CONTAINER
```

Get restart policy:

```bash
docker inspect -f '{{.HostConfig.RestartPolicy.Name}}' CONTAINER
```

Get image used by container:

```bash
docker inspect -f '{{.Config.Image}}' CONTAINER
```

---

# 8. Resource Usage

Live resource usage:

```bash
docker stats
```

Specific container:

```bash
docker stats CONTAINER
```

One-time resource snapshot:

```bash
docker stats --no-stream
```

Processes inside container:

```bash
docker top CONTAINER
```

---

# 9. Copy Files

Host to container:

```bash
docker cp ./file.txt CONTAINER:/app/file.txt
```

Container to host:

```bash
docker cp CONTAINER:/app/file.txt ./
```

Copy directory:

```bash
docker cp ./folder CONTAINER:/app/
```

---

# 10. Images

List images:

```bash
docker images
```

Equivalent modern command:

```bash
docker image ls
```

List all images:

```bash
docker images -a
```

Pull image:

```bash
docker pull nginx
```

Pull explicit latest tag:

```bash
docker pull nginx:latest
```

Pull specific version:

```bash
docker pull mysql:8.4
```

Remove image:

```bash
docker rmi IMAGE
```

Alternative:

```bash
docker image rm IMAGE
```

Force remove image:

```bash
docker rmi -f IMAGE
```

Inspect image:

```bash
docker image inspect IMAGE
```

Image history:

```bash
docker history IMAGE
```

Remove dangling images:

```bash
docker image prune
```

Remove all unused images:

```bash
docker image prune -a
```

---

# 11. Build Images

Build current directory:

```bash
docker build .
```

Build and tag:

```bash
docker build -t myapp .
```

Build versioned image:

```bash
docker build -t myapp:1.0 .
```

Use custom Dockerfile:

```bash
docker build -f Dockerfile.production -t myapp:production .
```

Build without cache:

```bash
docker build --no-cache -t myapp .
```

Pass build argument:

```bash
docker build --build-arg APP_ENV=production -t myapp .
```

Detailed build output:

```bash
docker build --progress=plain .
```

---

# 12. Tag Images

Tag image:

```bash
docker tag SOURCE TARGET
```

Docker Hub example:

```bash
docker tag myapp:latest username/myapp:latest
```

Private registry example:

```bash
docker tag myapp:latest registry.example.com/myapp:latest
```

General image naming format:

```text
[HOST[:PORT]/]NAMESPACE/REPOSITORY[:TAG]
```

---

# 13. Registry & Docker Hub

Login:

```bash
docker login
```

Logout:

```bash
docker logout
```

Login to private registry:

```bash
docker login registry.example.com
```

Push image:

```bash
docker push username/myapp:latest
```

Pull image:

```bash
docker pull username/myapp:latest
```

---

# 14. Save & Load Images

Save image to tar:

```bash
docker save nginx:latest > nginx.tar
```

Save using output option:

```bash
docker save -o nginx.tar nginx:latest
```

Load image:

```bash
docker load < nginx.tar
```

Load with input option:

```bash
docker load -i nginx.tar
```

Save compressed:

```bash
docker save nginx:latest | gzip > nginx.tar.gz
```

Load compressed image:

```bash
gunzip -c nginx.tar.gz | docker load
```

---

# 15. Volumes

List volumes:

```bash
docker volume ls
```

Create volume:

```bash
docker volume create app_data
```

Inspect volume:

```bash
docker volume inspect app_data
```

Remove volume:

```bash
docker volume rm app_data
```

Remove unused volumes:

```bash
docker volume prune
```

Run container with named volume:

```bash
docker run -v app_data:/var/lib/app IMAGE
```

Inspect volume mount path:

```bash
docker volume inspect app_data
```

---

# 16. Networks

List networks:

```bash
docker network ls
```

Create network:

```bash
docker network create app-network
```

Inspect network:

```bash
docker network inspect app-network
```

Remove network:

```bash
docker network rm app-network
```

Remove unused networks:

```bash
docker network prune
```

Connect container:

```bash
docker network connect app-network CONTAINER
```

Disconnect container:

```bash
docker network disconnect app-network CONTAINER
```

Run container directly in network:

```bash
docker run --network app-network nginx
```

Create explicit bridge network:

```bash
docker network create --driver bridge app-network
```

---

# 17. Docker Disk Usage

Show Docker disk usage:

```bash
docker system df
```

Detailed disk usage:

```bash
docker system df -v
```

---

# 18. Docker Cleanup

Remove stopped containers:

```bash
docker container prune
```

Remove dangling images:

```bash
docker image prune
```

Remove all unused images:

```bash
docker image prune -a
```

Remove unused networks:

```bash
docker network prune
```

Remove unused volumes:

```bash
docker volume prune
```

General cleanup:

```bash
docker system prune
```

More aggressive cleanup:

```bash
docker system prune -a
```

Cleanup including unused volumes:

```bash
docker system prune -a --volumes
```

Check disk usage before cleanup:

```bash
docker system df -v
```

> **Warning:** Be very careful with volume cleanup. Volumes may contain databases or persistent application data.

---

# 19. Docker Compose: Start

Start services:

```bash
docker compose up
```

Start in background:

```bash
docker compose up -d
```

Build and start:

```bash
docker compose up -d --build
```

Force recreation:

```bash
docker compose up -d --force-recreate
```

---

# 20. Docker Compose: Status

Show services:

```bash
docker compose ps
```

Show all services:

```bash
docker compose ps -a
```

Show images:

```bash
docker compose images
```

Show processes:

```bash
docker compose top
```

Show resource usage:

```bash
docker compose stats
```

---

# 21. Docker Compose: Stop / Start / Restart

Stop services:

```bash
docker compose stop
```

Start stopped services:

```bash
docker compose start
```

Restart services:

```bash
docker compose restart
```

Restart one service:

```bash
docker compose restart app
```

Stop MySQL service:

```bash
docker compose stop mysql
```

Start MySQL service:

```bash
docker compose start mysql
```

---

# 22. Docker Compose: Down

Stop and remove Compose resources:

```bash
docker compose down
```

Remove volumes too:

```bash
docker compose down -v
```

Remove images:

```bash
docker compose down --rmi all
```

Remove containers, volumes, and images:

```bash
docker compose down -v --rmi all
```

> **Warning:** `-v` removes project volumes and can delete persistent data.

---

# 23. Docker Compose: Logs

Show logs:

```bash
docker compose logs
```

Follow logs:

```bash
docker compose logs -f
```

Last 100 lines:

```bash
docker compose logs --tail=100
```

Follow recent logs:

```bash
docker compose logs -f --tail=200
```

Specific service:

```bash
docker compose logs app
```

Follow Nginx logs:

```bash
docker compose logs -f nginx
```

Follow MySQL logs:

```bash
docker compose logs -f mysql
```

---

# 24. Docker Compose: Exec / Shell

Open Bash in service:

```bash
docker compose exec app bash
```

Open `sh`:

```bash
docker compose exec app sh
```

Laravel migration:

```bash
docker compose exec app php artisan migrate
```

Laravel optimize:

```bash
docker compose exec app php artisan optimize
```

Horizon status:

```bash
docker compose exec app php artisan horizon:status
```

Restart Laravel queues:

```bash
docker compose exec app php artisan queue:restart
```

---

# 25. Docker Compose: One-off Commands

Run a command in a temporary container:

```bash
docker compose run app COMMAND
```

Laravel migration:

```bash
docker compose run --rm app php artisan migrate
```

Composer install:

```bash
docker compose run --rm app composer install
```

Node build:

```bash
docker compose run --rm node npm run build
```

Difference:

```text
docker compose exec
→ Runs inside an existing running container.

docker compose run
→ Creates a temporary container for a one-off command.
```

---

# 26. Docker Compose: Build

Build all services:

```bash
docker compose build
```

Build one service:

```bash
docker compose build app
```

Build without cache:

```bash
docker compose build --no-cache
```

Pull newer base images while building:

```bash
docker compose build --pull
```

Start afterward:

```bash
docker compose up -d
```

Common deployment sequence:

```bash
docker compose pull
```

```bash
docker compose up -d --remove-orphans
```

---

# 27. Docker Compose: Validate Config

Render and validate config:

```bash
docker compose config
```

Quiet validation:

```bash
docker compose config -q
```

Show defined services:

```bash
docker compose config --services
```

---

# 28. Docker Compose: Multiple Files

Use a custom Compose file:

```bash
docker compose -f docker-compose.production.yml up -d
```

Use multiple Compose files:

```bash
docker compose -f compose.yml -f compose.prod.yml up -d
```

---

# 29. Buildx

Check Buildx version:

```bash
docker buildx version
```

List builders:

```bash
docker buildx ls
```

Create and use builder:

```bash
docker buildx create --name mybuilder --use
```

Inspect builder:

```bash
docker buildx inspect
```

Multi-platform build:

```bash
docker buildx build --platform linux/amd64,linux/arm64 -t username/myapp:latest --push .
```

---

# 30. Ports

Show container port mappings:

```bash
docker port CONTAINER
```

Example:

```bash
docker port nginx
```

Formatted container ports:

```bash
docker ps --format 'table {{.Names}}\t{{.Ports}}'
```

---

# 31. Docker Events

Watch Docker events:

```bash
docker events
```

Events from last hour:

```bash
docker events --since 1h
```

---

# 32. Filters

Running containers only:

```bash
docker ps --filter status=running
```

Exited containers only:

```bash
docker ps --filter status=exited
```

Filter by name:

```bash
docker ps --filter name=app
```

Dangling images:

```bash
docker images --filter dangling=true
```

---

# 33. Formatted Output

Container name and status:

```bash
docker ps --format '{{.Names}} {{.Status}}'
```

Useful container table:

```bash
docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}'
```

Useful image table:

```bash
docker images --format 'table {{.Repository}}\t{{.Tag}}\t{{.Size}}'
```

---

# 34. Restart Policies

No automatic restart:

```bash
docker run --restart=no IMAGE
```

Always restart:

```bash
docker run --restart=always IMAGE
```

Restart unless manually stopped:

```bash
docker run --restart=unless-stopped IMAGE
```

Restart on failure:

```bash
docker run --restart=on-failure IMAGE
```

Update existing container:

```bash
docker update --restart=unless-stopped CONTAINER
```

---

# 35. Resource Limits

Update memory:

```bash
docker update --memory 2g CONTAINER
```

Update CPU:

```bash
docker update --cpus 2 CONTAINER
```

Inspect limits:

```bash
docker inspect CONTAINER
```

---

# 36. Docker Context

List contexts:

```bash
docker context ls
```

Show active context:

```bash
docker context show
```

Switch to default:

```bash
docker context use default
```

Create remote context over SSH:

```bash
docker context create production --docker "host=ssh://user@server"
```

Use remote context:

```bash
docker context use production
```

Now Docker commands target that server:

```bash
docker ps
```

Return to local Docker:

```bash
docker context use default
```

---

# 37. Docker Socket & Permissions

Inspect Docker socket:

```bash
ls -l /var/run/docker.sock
```

Add current user to Docker group:

```bash
sudo usermod -aG docker $USER
```

Apply group immediately:

```bash
newgrp docker
```

Test Docker access:

```bash
docker ps
```

> **Security note:** Membership in the `docker` group effectively grants root-level access to the machine.

---

# 38. Docker Daemon Logs

Show daemon logs:

```bash
sudo journalctl -u docker
```

Follow daemon logs:

```bash
sudo journalctl -u docker -f
```

Last 100 lines:

```bash
sudo journalctl -u docker -n 100
```

Current boot only:

```bash
sudo journalctl -u docker -b
```

---

# 39. Docker Storage Paths

Typical Docker data directory:

```text
/var/lib/docker
```

Show total size:

```bash
sudo du -sh /var/lib/docker
```

Show storage by top-level directory:

```bash
sudo du -h --max-depth=1 /var/lib/docker | sort -h
```

Prefer Docker's own cleanup commands instead of deleting files manually:

```bash
docker system df
```

```bash
docker system prune
```

---

# 40. Find Large Docker Logs

Find large JSON container logs:

```bash
sudo find /var/lib/docker/containers -name '*-json.log' -exec du -h {} + | sort -h
```

---

# 41. Production Troubleshooting

Running containers:

```bash
docker ps
```

All containers:

```bash
docker ps -a
```

Resource usage:

```bash
docker stats
```

Follow recent logs:

```bash
docker logs -f --tail 200 CONTAINER
```

Inspect container:

```bash
docker inspect CONTAINER
```

Disk usage:

```bash
docker system df
```

Networks:

```bash
docker network ls
```

Volumes:

```bash
docker volume ls
```

Docker daemon logs:

```bash
sudo journalctl -u docker -n 200
```

Compose service status:

```bash
docker compose ps
```

Compose logs:

```bash
docker compose logs -f --tail 200
```

Validate Compose config:

```bash
docker compose config
```

---

# 42. Most-used Commands

These cover a large percentage of everyday DevOps work:

```bash
docker ps
```

```bash
docker ps -a
```

```bash
docker images
```

```bash
docker logs -f --tail 200 CONTAINER
```

```bash
docker exec -it CONTAINER bash
```

```bash
docker exec -it CONTAINER sh
```

```bash
docker inspect CONTAINER
```

```bash
docker stats
```

```bash
docker stop CONTAINER
```

```bash
docker start CONTAINER
```

```bash
docker restart CONTAINER
```

```bash
docker rm CONTAINER
```

```bash
docker rmi IMAGE
```

```bash
docker network ls
```

```bash
docker volume ls
```

```bash
docker system df
```

```bash
docker compose ps
```

```bash
docker compose up -d
```

```bash
docker compose up -d --build
```

```bash
docker compose down
```

```bash
docker compose logs -f
```

```bash
docker compose exec app bash
```

```bash
docker compose pull
```

```bash
docker compose restart
```

```bash
docker compose config
```

---

# 43. Deployment Quick Flow

Pull application changes:

```bash
git pull
```

Pull latest container images:

```bash
docker compose pull
```

Build services:

```bash
docker compose build
```

Start/recreate services:

```bash
docker compose up -d --remove-orphans
```

Check service status:

```bash
docker compose ps
```

Review recent logs:

```bash
docker compose logs --tail=100
```

Laravel example:

```bash
docker compose exec app php artisan migrate --force
```

```bash
docker compose exec app php artisan optimize
```

```bash
docker compose exec app php artisan queue:restart
```

---

# 44. Official Docker References

Official Docker CLI reference:

```text
https://docs.docker.com/reference/cli/docker/
```

Official Docker Compose reference:

```text
https://docs.docker.com/reference/cli/docker/compose/
```

Official Docker cheat sheet PDF:

```text
https://docs.docker.com/get-started/docker_cheatsheet.pdf
```

Docker Engine Linux post-install guide:

```text
https://docs.docker.com/engine/install/linux-postinstall/
```

Docker networking reference:

```text
https://docs.docker.com/reference/cli/docker/network/
```

Docker volume reference:

```text
https://docs.docker.com/reference/cli/docker/volume/
```

Docker system reference:

```text
https://docs.docker.com/reference/cli/docker/system/
```

---

## Safety Notes

Before destructive cleanup commands, inspect Docker disk usage:

```bash
docker system df -v
```

Be cautious with:

```bash
docker volume prune
```

```bash
docker system prune -a --volumes
```

```bash
docker compose down -v
```

These commands may remove persistent data such as MySQL, PostgreSQL, Redis, uploads, or application state if that data lives in Docker volumes.

---

## Suggested Aliases

Optional shortcuts for your shell:

```bash
alias dps='docker ps'
```

```bash
alias dpa='docker ps -a'
```

```bash
alias di='docker images'
```

```bash
alias dc='docker compose'
```

```bash
alias dcu='docker compose up -d'
```

```bash
alias dcd='docker compose down'
```

```bash
alias dcl='docker compose logs -f --tail=200'
```

```bash
alias dcp='docker compose ps'
```

```bash
alias ddf='docker system df'
```

Add aliases permanently to Bash:

```bash
nano ~/.bashrc
```

Reload:

```bash
source ~/.bashrc
```

---

**End of Docker CLI Cheat Sheet**
