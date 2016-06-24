# Filebeat 1.2.1 Docker image
Filebeat Docker image based on Ubuntu 14.04

This image expose 6000 and 9000 ports and use a user defined config file.
Config file filebeat.yml contains a base configuration with two Logastash hosts and loadbalancing, replace Logastash hosts addresses with your configuration.

### Build
```sh
sudo docker build -t filebeat .
```
Replace "filebeat" with your tag (eg. myregistr/filebeat)

### Run
On each host (or use Swarm with constrain:node) run
```sh
sudo docker run -d --name=<nodename> -h <nodename> --restart=always \
        --net=<elasticsearch_network> \
        -p 6000:6000 \
        -v <config_path>/filebeat.yml:/etc/filebeat/filebeat.yml \
        -v /var/log:/logs \
        -v /var/lib/docker/containers/:/dockerlogs \
        filebeat
```
Replace nodename with filebeat00, filebeat01, etc. This is important for distinguish hosts while using Kibana.
elasticsearch_network is the name of the overlay network used by Elastisearch cluster.
