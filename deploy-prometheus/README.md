To monitor k8s, you only need to have a running Kubernetes cluster with deployed Prometheus. Prometheus will use metrics provided by cAdvisor via kubelet service (runs on each node of Kubernetes cluster by default) and via kube-apiserver service only.

PREPARATION
Assume that you have:
•	Kubernetes cluster up and running.
•	Grafana instance up and running (inside or outside cluster).

In this sample, we run Prometheus pod inside Kubernetes cluster 
•	Run the Prometheus pod on the Kubernetes cluster.
•	Get metrics in a cluster with Prometheus pods.
•	Visualize metrics with Grafana outside cluster.
•	The retention period is 1 day for data acquired by Prometheus pod.
 
CREATE A NAMESPACE
[root@master ~]# kubectl create namespace monitoring

CREATE CLUSTER ROLE
[root@master ~]# kubectl create -f clusterRole.yaml

CREATE A CONFIG MAP
[root@master ~]# kubectl create -f config-map.yaml -n monitoring

CREATE PERSISTEN VOLUME FOR PROMETHUS
[root@master ~]# kubectl create -f prometheus-storage-volume-pv.yaml
[root@master ~]# kubectl create -f prometheus-storage-volume-pvc.yaml

CREATE A PROMETHEUS DEPLOYMENT
[root@master ~]# kubectl create -f prometheus-deployment.yaml

EXPOSING PROMETHEUS AS A SERVICE
[root@master ~]# kubectl create -f prometheus-service.yaml --namespace=monitoring
