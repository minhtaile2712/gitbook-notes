# Kubernetes

**Rancher**: manage Kubernetes at scale. It addresses the operational and security challenges of managing multiple Kubernetes clusters across any infrastructure.

**Helm**: package manager for Kubernetes.

```
K8s cluster
  Control Plane
    kube-apiserver
    etcd
    kube-scheduler
    kube-controller-manager
    cloud-controller-manager
  Node(s)
    kubelet
    kube-proxy
    Container runtime (containerd,...)
  Addons
    DNS
    Web UI (Dashboard)
    Container Resource Monitoring
    Cluster-level Logging
```

Nodes: virtual or physical machine.

Pod: group of Docker containers with shared namespaces and shared filesystem volumes.

```
kubectl cluster-info

kubectl get <anything>
kubectl describe <anything>
kubectl exec [-ti] <pod> -- <command>
kubectl logs <pod>
kubectl describe <anything>

kubectl apply -f pod.yaml
kubectl scale deployments/kubernetes-bootcamp --replicas=4
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2
kubectl label pods $POD_NAME version=v1
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080

kubectl rollout status deployments/kubernetes-bootcamp
kubectl rollout undo deployments/kubernetes-bootcamp

kubectl delete -f pod.yaml
kubectl delete service -l app=kubernetes-bootcamp

export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')
export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')
```

op

```
kubectl apply -f pod.yaml
kubectl delete -f pod.yaml
```
