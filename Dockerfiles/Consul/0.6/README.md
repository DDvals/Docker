# Consul 0.6.4 Docker image

Consul Docker image based on Ubuntu 14.04 LTS


The image expose all Consul's ports and create a folder called "data" to
store container's data

Consul's standard ports:
```
8300 8301 8301/udp 8302 8302/udp 8400 8500 8600 53 53/udp
```

## Build

To build the image put Dockerfile and config folder in a folder and run

```sh
sudo docker build -t consul .
```

Replace "consul" with your tag (eg. myregistry/consul)

## Run

To run the image in server mode launch

```sh
sudo docker run -d --name=<container_name> -h <consul_node_name> \
    -v /mnt/consul_data:/data --restart=always \
    -p 8300:8300 \
    -p 8301:8301 \
    -p 8301:8301/udp \
    -p 8302:8302 \
    -p 8302:8302/udp \
    -p 8400:8400 \
    -p 8500:8500 \
    -p 8600:8600 \
    -p 8600:8600/udp \
    -p 53:53 \
    -p 53:53/udp \
    consul -server -advertise <node_ip> -bootstrap-expect <num_consul>
```

 <node_ip> indicates the ip adress of the host

 Replace <num_consul> with the number of Consul server needed in the
 cluster, to work properly a Consul cluster needs at least 3 nodes and
 only one can fail.

### Replicas setup

 ```
 sudo docker run -d --name=<container_name> -h <consul_node_name> \
    -v /mnt/consul_data:/data --restart=always \
    -p 8300:8300 \
    -p 8301:8301 \
    -p 8301:8301/udp \
    -p 8302:8302 \
    -p 8302:8302/udp \
    -p 8400:8400 \
    -p 8500:8500 \
    -p 8600:8600 \
    -p 8600:8600/udp \
    -p 53:53 \
    -p 53:53/udp \
    consul -server -advertise <node_ip> -join <existent_consul_ip>
```

 Replace <existent_consul_ip> with the ip adress of one node of the
 cluster, not necessary the master node.

### Clients setup

 ```
 sudo docker run -d --name=<container_name> -h <consul_node_name> \
    -v /mnt/consul_data:/data --restart=always \
    -p 8300:8300 \
    -p 8301:8301 \
    -p 8301:8301/udp \
    -p 8302:8302 \
    -p 8302:8302/udp \
    -p 8400:8400 \
    -p 8500:8500 \
    -p 8600:8600 \
    -p 8600:8600/udp \
    -p 53:53 \
    -p 53:53/udp \
    consul -advertise <node_ip> -join <existent_consul_ip>
```

### Check status

To check the status of the cluster run
```
sudo docker exec <consul_container> consul members
```

If all works properly all nodes will be marked as alive.

To retrieve node information run
```
sudo docker exec <consul_container> consul info
```

If Consul tells that can't connect to UDP port of another client
run on the other client
```sh
sudo conntrack -D -p udp
```

### DNS service

Consul provides DNS service. To use the service with container
add
```sh
--dns <docker0_ip> --dns <yourdns>
```

to the string DOCKER_OPTS in /etc/default/docker and reboot the daemon

If needed in the consul config file replace recursor ip with your internal DNS service address

To test if it works run
```sh
dig @<consul_ip> consul.service.consul
```
This should print consul node's IPs
