# Docker Quickstart

The container image bundles Asterisk 20 LTS with PJSIP support. Use the snippets below to build, run, and restart the service locally.

## Build

```bash
docker build -t pbx-for-dummies .. -f docker/Dockerfile
```

## Run

```bash
docker run -d --name pbx \
  -p 5060:5060/udp \
  -p 5061:5061/tcp \
  -p 10000-20000:10000-20000/udp \
  -v pbx-config:/etc/asterisk \
  -v pbx-log:/var/log/asterisk \
  pbx-for-dummies
```

## Restart

```bash
docker restart pbx
```

### Compose helper

```bash
chmod +x docker/run_docker.sh
./docker/run_docker.sh
```

The script wraps `docker compose up --build -d` against `docker/docker-compose.yml`. Use `docker logs pbx` to follow Asterisk console output when debugging registrations or calls.
