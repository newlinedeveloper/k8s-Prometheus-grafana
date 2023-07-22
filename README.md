# k8s-Prometheus-grafana
Kubernetes monitoring using prometheus and Grafana

Grafana and Prometheus Kubernetes Cluster Monitoring reports on potential performance bottlenecks, cluster health, and performance metrics. Simultaneously, visualize network usage, pod resource usage patterns, and a high-level overview of what’s going on in your cluster.

### Prerequisites

- Docker setup: https://docs.docker.com/engine/install/
- Kubectl installation: https://kubernetes.io/docs/tasks/tools/
- Minikube setup : https://minikube.sigs.k8s.io/docs/start/
- helm setup  - https://helm.sh/docs/intro/install/

```
docker version

kubectl version

minikube version

helm version

minikube start

```


###  Implementation

```
kubectl create namespace kubernetes-monitoring  

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts


helm repo update

helm install prometheus prometheus-community/kube-prometheus-stack -n kubernetes-monitoring

kubectl get pods -n kubernetes-monitoring

kubectl get svc -n kubernetes-monitoring

kubectl get secret prometheus-grafana -n kubernetes-monitoring -o jsonpath="{.data.admin-password}" | base64 --decode ; echo

kubectl get secret prometheus-grafana -n kubernetes-monitoring -o jsonpath="{.data.admin-user}" | base64 --decode ; echo


kubectl port-forward svc/prometheus-kube-prometheus-prometheus -n kubernetes-monitoring 9090

kubectl port-forward svc/prometheus-kube-state-metrics -n kubernetes-monitoring 8080

kubectl port-forward svc/prometheus-grafana -n kubernetes-monitoring 3001:80

```

#### Basic PromQL queries for Prometheus

- Query to get the average CPU usage across all instances:

```
avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100
```

- Query to get the total memory usage across all instances:

```
sum(node_memory_MemTotal_bytes) - sum(node_memory_MemFree_bytes + node_memory_Cached_bytes + node_memory_Buffers_bytes)
```
- Query to get the total number of pods in the Kubernetes cluster:

```
count(kube_pod_info)
```

### Example Alert Queries

- last disk usage > 90 %

```
100 - (node_filesystem_avail_bytes{fstype="ext4"} / node_filesystem_size_bytes{fstype="ext4"}) * 100 > 90

```

- avg mem > 90% for 5 mintues

```
avg_over_time(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes{instance="your_instance"}[5m]) / avg_over_time(node_memory_MemTotal_bytes{instance="your_instance"}[5m]) * 100 > 90
```

- Avg temp outside (10,30) for 10 mintues
```
avg_over_time(outside_temperature{location="your_location"}[10m])

```

#### Troubleshooting for Alert configuration

**SMTP Configuration for Email noficiation**


```
kubectl get deployments -n kubernetes-monitoring

kubectl edit deploy prometheus-grafana -n kubernetes-monitoring

```

add env to grafana container

```
- env:
 — name: GF_SMTP_ENABLED
 value: “true”
 — name: GF_SMTP_HOST
 value: localhost:587
 — name: GF_SMTP_PASSWORD
 value: xxxxxxxxxxxx
 — name: GF_SMTP_USER
 value: apikey
 — name: GF_SMTP_FROM_ADDRESS
 value: test@gmail.com
 — name: GF_SMTP_FROM_NAME
 value: System Admin

```




