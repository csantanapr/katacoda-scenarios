## Install Networking Layer

Startig with Knative version `0.13` you can choose from multiple networing layers like Istio, Contour, Kourier, and Ambasador.
More info [#installing-the-serving-component](https://knative.dev/docs/install/any-kubernetes-cluster/#installing-the-serving-component)

```
kubectl apply --filename https://github.com/knative/net-kourier/releases/download/v0.15.0/kourier.yaml
```{{execute}}

Verify Kourier is Running
```
watch kubectl get pods --namespace kourier-system
```{{execute}}



```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: kourier-nodeport
  namespace: kourier-system
  labels:
    networking.knative.dev/ingress-provider: kourier
spec:
  ports:
  - name: http2
    port: 80
    protocol: TCP
    targetPort: 8080
    nodePort: 32080
  - name: https
    port: 443
    protocol: TCP
    targetPort: 8443
    nodePort: 32443
  selector:
    app: 3scale-kourier-gateway
  type: NodePort
EOF
```{{execute}}


Verify the Kourier Service for NodePort
```
kubectl get svc kourier-nodeport -n kourier-system
```{{execute}}

```
kubectl patch configmap/config-network \
  --namespace knative-serving \
  --type merge \
  --patch '{"data":{"ingress.class":"kourier.ingress.networking.knative.dev"}}'
```{{execute}}

## Configure DNS
`echo [[HOST_IP]]`{{execute}}

<pre>[[HOST_IP]]</pre>

## URL

http://[[HOST_SUBDOMAIN]]-32000-[[KATACODA_HOST]].environments.katacoda.com

https://[[HOST_SUBDOMAIN]]-32443-[[KATACODA_HOST]].environments.katacoda.com

`echo [[HOST_SUBDOMAIN]]-32000-[[KATACODA_HOST]].[[KATACODA_DOMAIN]]`{{execute}}

<pre>[[HOST_SUBDOMAIN]]-32000-[[KATACODA_HOST]].[[KATACODA_DOMAIN]]</pre>

<pre>[[HOST_SUBDOMAIN]]</pre>

<pre>[[KATACODA_HOST]]</pre>

<pre>[[KATACODA_DOMAIN]]</pre>

```
export KNATIVE_DOMAIN="[[HOST_SUBDOMAIN]]-32000-[[KATACODA_HOST]].environments.katacoda.com"
```{{execute}}

```
kubectl patch configmap -n knative-serving config-domain -p "{\"data\": {\"$KNATIVE_DOMAIN\": \"\"}}"
```{{execute}}
