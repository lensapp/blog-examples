## Prometheus and Grafana example

### Prerequisites
For this example, you will need: 
- A Kubernetes cluster (Minikube works just fine): installation guide for Minikube is available here
- Lens Kubernetes IDE for better management of everything that is happening: You can download Lens from here.
- kubectl
- helm


### Installing the Prometheus Stack Helm Chart
We will use the kube-prometheus-stack Helm chart from the Prometheus community. It includes Prometheus, Grafana, and Alertmanager, all managed by the Prometheus Operator.

To add the Helm repository simply run:
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Create a namespace called _monitoring_ in which you will install the **kube-prometheus-stack** Helm chart.

```
kubectl create ns monitoring
```

Install the chart using Lens, and make changes to the alertmanager values based on this [file](./alertmanager-values.yaml)

Make sure you create a Slack application to receive alerts from alertmanager when certain conditions happen. Copy the WebHook URL and use it in the above file.

More details on how to do this are available [here](https://k8slens.dev/blog/prometheus-and-grafana).

Ensure your node has more than 11 pods, and then create the pods alert:
```
kubectl apply -f pods.yaml
```

You should now receive an alert in slack.