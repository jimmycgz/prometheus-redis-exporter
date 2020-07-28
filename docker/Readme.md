# Docker version for single target without Grafana
```
export dk_network=dk_network_test
export redis_addr=dk_redis_test
docker network create $dk_network --driver bridge

# Run redis within the network
docker run -it -d --network $dk_network --rm --name $redis_addr redis

# Test redis cli from another container, connecting to the above redis 
docker run -it --network $dk_network --rm redis redis-cli -h $redis_addr
dk_redis_test:6379>INFO

# Run exporter within the network, expose to local port, connecting to the above redis
docker run -d --rm --name redis_exporter --network $dk_network -p 9121:9121 bitnami/redis-exporter:latest --redis.addr=$redis_addr

# Access via localhost:9121 
curl localhost:9121/metrics
curl localhost:9121/metrics | grep redis_up
# Result: redis_up 1

# Stop redis target
docker stop $redis_addr
curl localhost:9121/metrics | grep redis_up
# Result: redis_up 0
```
