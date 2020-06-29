## Install Knative


Clone the Tutorial Git Repository
```
git clone https://github.com/csantanapr/knative-tekton.git
cd knative-tekton
```{{execute}}

Run the Script to Install Knative for the Tutorial
```
source .katacoda/knative.sh
```{{execute}}

Run this command until all pods are **Running**
```
kubectl get pods -n knative-serving
kubectl get pods -n kourier-system
```{{execute}}
