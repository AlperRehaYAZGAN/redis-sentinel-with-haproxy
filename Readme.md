# Build fault-tolerance Redis cluster with Sentinel on Docker  
originally forked from https://raw.githubusercontent.com/selcukusta/redis-sentinel-with-haproxy
## Topology
![topology](https://raw.githubusercontent.com/alperrehayazgan/redis-sentinel-with-haproxy/master/redis-sentinel-mode-topology.png)
## Single Node Cluster Setup  
It only required single command to run whole cluster:  
- `docker-compose -f docker-compose-single-node.yml up -d`  
  
## Multi Node Cluster Setup  
Lets suppose you have 3 machine to setup and all machine has a docker, docker-compose already. We will use machines for one of load balancing, one of master and one of slave machine. You should follow:  
1 - (ssh redis-master-machine-ip):  
```sh
$ export SENTINEL_QUORUM=2
$ export SENTINEL_DOWN_AFTER=1000
$ export SENTINEL_FAILOVER=5000  
$ cd setup-master  
$ docker-compose -f docker-compose-master.yml up -d
```

2 - (ssh redis-slave-machine-ip):  
```sh
$ export MASTER_HOST="redis-master-machine-ip"
$ export SENTINEL_QUORUM=2
$ export SENTINEL_DOWN_AFTER=1000
$ export SENTINEL_FAILOVER=5000  
$ cd setup-worker  
$ docker-compose -f docker-compose-worker.yml up -d
```  
  
2 - (ssh lb-machine-ip):  
YOU SHOULD CONFIGURE "MASTER" and "SLAVE" REDIS IP's on file "./config/haproxy.cfg" before running.  
```sh
$ cd config-haproxy  
$ vim haproxy.cfg # update the rows ends with "# !!! ENTER YOUR REDIS SERVER IP AND PORT HERE !!!"
$ # save and exit file.
$ cd ../loadbalancer
$ docker-compose -f docker-compose-lb.yml up -d
```  

You are ready to go!!! 👏🏻

## Tail sentinel logs
`clear && tail -f /var/log/sentinel.log`
## Get redis replication info per seconds
`while true; do redis-cli info replication; sleep 1; clear; done`