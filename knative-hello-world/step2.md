## Install Networking Layer

Startig with Knative version `0.13` you can choose from multiple networing layers like Istio, Contour, Kourier, and Ambasador.
More info [#installing-the-serving-component](https://knative.dev/docs/install/any-kubernetes-cluster/#installing-the-serving-component)

```
kubectl apply --filename https://github.com/knative/net-kourier/releases/download/v0.15.0/kourier.yaml
```{{execute}}

Verify Kourier is Running
```bash
kubectl get pods --namespace kourier-system -w
```

Get the `EXTERNAL-IP` for the kourier svc
```bash
kubectl get svc kourier -n kourier-system
```
