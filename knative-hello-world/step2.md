## Install Networking Layer

Startig with Knative version `0.13` you can choose from multiple networing layers like Istio, Contour, Kourier, and Ambasador.
More info [#installing-the-serving-component](https://knative.dev/docs/install/any-kubernetes-cluster/#installing-the-serving-component)

```
kubectl apply --filename https://github.com/knative/net-kourier/releases/download/v0.15.0/kourier.yaml
```{{execute}}

Verify that all pods for Knative serving are **Running**
```
watch kubectl get pods --namespace kourier-system
```{{execute interrupt}}

When all pods are **Running** then exit the watch command using <kbd>Ctrl</kbd>+<kbd>C</kbd>


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

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: kourier-ingress
  namespace: kourier-system
  labels:
    networking.knative.dev/ingress-provider: kourier
spec:
  ports:
  - name: http2
    port: 80
    protocol: TCP
    targetPort: 8080
  - name: https
    port: 443
    protocol: TCP
    targetPort: 8443
  selector:
    app: 3scale-kourier-gateway
  type: NodePort
  externalIPs:
    - [[HOST_IP]]
EOF
```{{execute}}



Verify the Kourier Service for NodePort
```
kubectl get svc kourier-ingress -n kourier-system
```{{execute}}

Configure Knative to use Kourier
```
kubectl patch configmap/config-network \
  --namespace knative-serving \
  --type merge \
  --patch '{"data":{"ingress.class":"kourier.ingress.networking.knative.dev"}}'
```{{execute}}

## Configure DNS
```
export KNATIVE_DOMAIN="[[KATACODA_HOST]].nip.io"
kubectl patch configmap -n knative-serving config-domain -p "{\"data\": {\"$KNATIVE_DOMAIN\": \"\"}}"
echo DNS Domain KNATIVE_DOMAIN is now configure
```{{execute}}
