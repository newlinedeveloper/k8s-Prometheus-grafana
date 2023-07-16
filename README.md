# k8s-Prometheus-grafana
Kubernetes monitoring using prometheus and Grafana

Grafana and Prometheus Kubernetes Cluster Monitoring reports on potential performance bottlenecks, cluster health, and performance metrics. Simultaneously, visualize network usage, pod resource usage patterns, and a high-level overview of what’s going on in your cluster.

### Prerequisites

- Docker setup: https://docs.docker.com/engine/install/
- Kubectl installation: https://kubernetes.io/docs/tasks/tools/
- Minikube setup : https://minikube.sigs.k8s.io/docs/start/
- helm setup  - https://helm.sh/docs/intro/install/


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

