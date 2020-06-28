## Create Knative Service


1. Using the Knative CLI `kn` deploy an application usig a Container Image
    ```
    kn service create hello --image gcr.io/knative-samples/helloworld-go
    ```{{execute}}
1. You can see list your service
    ```
    kn service list hello
    ```{{execute}}
1. Use curl to invoke the Application
    ```
    curl http://hello.$SUB_DOMAIN
    ```{{execute}}
1. You can watch the pods and see how they scale down to zero after http traffic stops to the url
    ```
    kubectl get pod -l serving.knative.dev/service=hello -w
    ```{{execute interrupt}}

    Output should look like this after a few seconds when http traffic stops:
    ```
    NAME                                     READY   STATUS
    hello-r4vz7-deployment-c5d4b88f7-ks95l   2/2     Running
    hello-r4vz7-deployment-c5d4b88f7-ks95l   2/2     Terminating
    hello-r4vz7-deployment-c5d4b88f7-ks95l   1/2     Terminating
    hello-r4vz7-deployment-c5d4b88f7-ks95l   0/2     Terminating
    ```

    When the pod **Terminates** then exit the watch command using <kbd>Ctrl</kbd>+<kbd>C</kbd>

    Some people call this **Serverless** 🎉 🌮 🔥

1. There are no more pods
    ```
    kubectl get pods
    ```{{execute}}
