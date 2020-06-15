## Install Knative

Very kubernetes is ready and that all pods are in _Running_ state
```
kubectl get pods -A
```{{execute}}


## Install Knative Serving

Install Serving crds
```
kubectl apply --filename https://github.com/knative/serving/releases/download/v0.15.1/serving-crds.yaml

kubectl apply --filename https://github.com/knative/serving/releases/download/v0.15.1/serving-core.yaml
```{{execute}}

Verify that all pods for Knative serving are **Running**
```
watch kubectl get pods --namespace knative-serving
```{{execute interrupt}}

When all pods are **Running** then exit the watch command using <kbd>Ctrl</kbd>+<kbd>C</kbd>
