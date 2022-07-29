docker network create portainer 
docker volume create portainer_data
docker volume create traefik
cd /var/lib/docker/volumes/traefik/_data/
edit traefik.yml

mkdir /var/lib/docker/volumes/traefik/_data/certs
chmod 600 /var/lib/docker/volumes/traefik/_data/certs

docker-compose up -d