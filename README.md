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
http://localhost:3000/d/AuSZocVMz/prometheus-redis-by-addr-and-host?orgId=1&refresh=30s&var-addr=&var-host=redis_exporter2:9121&from=now-1h&to=now
