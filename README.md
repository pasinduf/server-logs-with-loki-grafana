To see docker container logs  using grafana follow below steps

1. Install grafana/loki-docker-driver in server
   docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions

2. Configure Docker daemon to use Loki driver
     create file /etc/docker/daemon.json
     {
     "log-driver": "loki",
     "log-opts": {
       "loki-url": "http://localhost:5002/loki/api/v1/push",
       "loki-retries": "5",
       "loki-batch-size": "400"
      }
     }

3. Restart docker
    sudo systemctl restart docker

4. Update docker run commadn with logs config (inside jenkins/docker-compose file)
    docker run -d \
    --log-driver=loki \
    --log-opt loki-url="http://localhost:5002/loki/api/v1/push" \
    --log-opt labels=container_name \
    --name ${CONTAINER_NAME} \
    -p ${PORT}:${PORT} \
    ${DOCKER_IMAGE}:latest
