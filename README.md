# blog-examples

This repository contains code examples for https://k8slens.dev/blog.


## K8s Deployment Strategies
If you want to use these examples as they are from a minikube cluster, you will need to enable the ingress addon, and start a tunnel.

```
minikube addons enable ingress
minikube tunnel
```

To install the nginx ingress controller you can run:
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```



### Argo Rollouts installation

To install Argo Rollouts you can run:
```
kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml
```


Run the patch for the rollout command:
```
kubectl patch rollout web -n demos --type='json' -p='[{"op":"replace","path":"/spec/template/spec/volumes/0/configMap/name","value":"web-v2"}
]'
```


### Ramped Slow Rollout
kubectl patch deploy web --type='json' -p='[Something to patch]'
# Pause mid-rollout and resume after checks
kubectl rollout pause deploy/web
# Inspect logs or metrics
kubectl rollout resume deploy/web
kubectl rollout status deploy/web