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

You can watch the pods and see how they scale down to zero after http traffic stops to the url
```
kubectl get pod -l serving.knative.dev/service=hello -w
```{{execute interrupt}}

When the pod **Terminates** then exit the watch command using <kbd>Ctrl</kbd>+<kbd>C</kbd>

Run the App again
```
curl $(kubectl get ksvc hello -o jsonpath='{.status.url}')
```{{execute}}

And see how the pod are scaled from zero
```
kubectl get pod -l serving.knative.dev/service=hello
```{{execute interrupt}}
