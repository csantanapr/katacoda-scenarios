## Install Knative

Very kubernetes is ready and that all pods are in _Running_ state
```
kubectl cluster-info
kubectl get nodes
kubectl get pods -A
```{{execute}}


## Install Knative Serving

Install Serving crds
```
kubectl apply --filename https://github.com/knative/serving/releases/download/v0.15.1/serving-crds.yaml
```{{execute}}

Install the Serving core
```
kubectl apply --filename https://github.com/knative/serving/releases/download/v0.15.1/serving-core.yaml
```{{execute}}

Verify that all pods for Knative serving are Running
```
kubectl get pods --namespace knative-serving -w
```{{execute}}
