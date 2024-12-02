# Artifact Life Cycle

## Deployment
#### 1. Build a Docker Image
```shell
sudo docker build --no-cache=true -f ./docker/Dockerfile -t alc:v2.1 .
```

#### 2. Run the Docker Image
This project is required the mysql database and opensearch instance.
```shell
sudo docker run -d \
 -p 8080:8080 \
 -e Moqui_HOST= \
 -e Moqui_DB_HOST=127.0.0.1 \
 -e Moqui_DB_NAME=alc \
 -e Moqui_DB_USER=root \
 -e Moqui_DB_PASSWORD=root \
 -e Moqui_configuration_DB_Name=moqui_configuration \
 -e ELASTICSEARCH_HOST=127.0.0.1 \
 -e ELASTICSEARCH_USER=admin \
 -e ELASTICSEARCH_PASSWORD=<password> \
 -e TIME_ZONE= \
 --name alc \
 alc:v2.1
```
## OpenSearch setup
```shell
sudo docker run \
 -p 9200:9200 \
 -p 9600:9600 \
 -e OPENSEARCH_INITIAL_ADMIN_PASSWORD=<password> \
 -e "discovery.type=single-node" \
 --name opensearch-node 
opensearchproject/opensearch:2.18.0
```
