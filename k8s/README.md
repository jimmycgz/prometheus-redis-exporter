## Deploy 2 containers in a same pod for Redis Exporter and Redis.

#### File list
* redis-exporter-deploy-b4-v15.yaml might work for lower k8s versions
* redis-exporter-v18-tested.yaml is tested on k8s v1.18 after tailoring from the above file

#### Deployment
```
kuberctl apply -f ./redis-exporter-v18-tested.yaml
```

#### Test 
```
kg pods -n redis
kdesc redis pod redis-7479d9b867-t5chf
kg deploy -n redis

# check log for each container
klo redis redis-7479d9b867-t5chf -c redis
klo redis redis-7479d9b867-t5chf -c redis-exporter

# forward redis-exporter port to localhost
k port-forward redis-7479d9b867-t5chf 9121:9121 -n redis
curl localhost:9121/metrics | grep -v "#"
```
