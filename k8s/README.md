## Exercise 1 <DONE> : Deploy 2 containers in a same pod for Redis Exporter and Redis.

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
export pod_id=redis-7479d9b867-t5chf
kdesc redis pod $pod_id
kg deploy -n redis

# check log for each container
klo redis $pod_id -c redis
klo redis $pod_id -c redis-exporter

# Check metrics from inside redis container, 2 containers has the same localhost on pod level.
kubectl exec -i -t -n redis $pod_id -c redis -- bash
apt-get update && apt-get install -y curl
curl localhost:9121/metrics

# forward redis-exporter port to localhost
k port-forward $pod_id 9121:9121 -n redis
curl localhost:9121/metrics | grep -v "#"
```

## Exercise 2 <WIP>: Deploy 1 pod Redis_Exporter to scrap from ElastiCache
Refer to https://itnext.io/kubernetes-monitoring-with-prometheus-exporters-a-service-discovery-and-its-roles-ce63752e5a1
  
```
  #Deploy in AWS EKS
kaf exporter-EC.yaml
  
#Test from bash in the redis pod
kex prom <redis-pod> -- bash
```
  
## Exercise 3 <WIP>: Deploy 3 pods in a same namespace:Redis_Exporter, Redis and Prometheus

* Generate configmap yaml 
```
kubectl create configmap prometheus-example-cm --from-file ./prometheus.yml
kg cm prometheus-example-cm -o yaml
# Copy/paste the yaml content to redis-prom.yaml
k delete cm prometheus-example-cm -n default
```
* Launch all pods 
```
kaf redis-prom.yaml
```
#### Known issue:
The scraping job config doesn't work, prom can't find the right redis pods. Two options to resolve this:

Option 1: Create service for redis target, and scrap svc endpoint as stated at below post, 
https://itnext.io/kubernetes-monitoring-with-prometheus-exporters-a-service-discovery-and-its-roles-ce63752e5a1

Option 2: See below for how to bind the cluster role to filter pods like below solution:
https://stackoverflow.com/questions/53908848/kubernetes-pods-nodes-is-forbidden

