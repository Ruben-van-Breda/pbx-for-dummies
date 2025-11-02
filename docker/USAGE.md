# Running the Dockerized Asterisk

## Build and start

```bash
chmod +x docker/run_docker.sh   # first run only
./docker/run_docker.sh
```

The helper script wraps `docker compose up --build -d` using `docker/docker-compose.yml`.

## Inspect the running service

```bash
docker ps --filter name=pbx
docker logs -f pbx
docker exec -it pbx asterisk -rvvv
```

Use the remote CLI (`asterisk -rvvv`) to run commands such as `pjsip show endpoints` or `core show modules`.

## Restart or stop

```bash
docker compose -f docker/docker-compose.yml restart
docker compose -f docker/docker-compose.yml down
```

To reload the Asterisk core within the container without restarting Docker:

```bash
docker exec pbx asterisk -rx "core restart now"
```
