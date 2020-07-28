# Learn Prometheus/Grafana from Zero to Hero

Steps to Implement Redis Exporter, integrating with Prometheus, Grafana, to scrap metrics for all kinds of redis instances: EC2, Pod, ElastiCache by endpoint

* Docker version
* Docker-compose
* K8S
* Helm

## Test single target (Redis container) from your local docker, without Grafana
See readme from docker folder

## Test multiple targets (Redis containers) by docker compose

### Setup steps
```
cd docker-compose
docker-compose up

# Check prometheus
curl localhost:9090/metrics | grep redis
# Result: prometheus_sd_discovered_targets{config="redis_exporter",name="scrape"} 2

# Check redis_exporter1
curl localhost:19121/metrics | grep redis
```

### Setup Grafana
Config datasource: http://prometheus:9090
Import dashboard id 4074 specifying datasource as Prometheus
View metrics via below link
http://localhost:3000/d/AuSZocVMz/prometheus-redis-by-addr-and-host?orgId=1&refresh=30s&from=now-15m&to=now&var-addr=&var-host=redis_exporter1:9121


### REMARKS

Set below command for redis_exporter to avoid scraping from localhost
```
 --redis.addr=
```
port 19121/16379/9090 are accessible via 
```
curl localhost:port
telnet localhost port
```

port 9121 6379 9090 are accessible via any container inside within the default network defined by docker-compose
