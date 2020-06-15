## Deploy Knative Application

Deploy a Knative Service using the following yaml manifest:

```
cat <<EOF | kubectl apply -f -
apiVersion: serving.knative.dev/v1
kind: Service
metadata:
  name: hello
spec:
  template:
    spec:
      containers:
        - image: gcr.io/knative-samples/helloworld-go
          ports:
            - containerPort: 8080
          env:
            - name: TARGET
              value: "Knative"
EOF
```{{execute}}



Verify status of Knative Service until is Ready
```
kubectl get ksvc -w
```{{execute interrupt}}

You can also exit the watch command with <kbd>Ctrl</kbd>+<kbd>C</kbd>

Test the App
```
curl $(kubectl get ksvc hello -o jsonpath='{.status.url}')
```{{execute}}
