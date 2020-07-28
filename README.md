# Steps to Implement Redis Exporter, integrating with Prometheus, Grafana

## Test single target from your local docker, without Grafana
See readme from docker folder

## Test multiple targets by docker compose

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
port 19121/16379/9090 are accessible via 
```
curl localhost:port
telnet localhost port
```

port 9121 6379 9090 are accessible via any container inside within the default network defined by docker-compose
