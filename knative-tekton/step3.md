## Install Tekton

Run the Script to Install Knative for the Tutorial
```
./.katacoda/tekton.sh
```{{execute}}



- Get access to a container registry such as quay, dockerhub, or your own private registry instance from a Cloud provider such as IBM Cloud 😉. On this tutorial we are going to use [Dockerhub](https://hub.docker.com/)

1 Set the environment variables `REGISTRY_SERVER`, `REGISTRY_NAMESPACE` and `REGISTRY_PASSWORD`, The `REGISTRY_NAMESPACE` most likely would be your dockerhub username. For Dockerhub use `docker.io` as the value for `REGISTRY_SERVER`
    ```
    REGISTRY_SERVER='docker.io'
    REGISTRY_NAMESPACE='REPLACEME_DOCKER_USERNAME_VALUE'
    REGISTRY_PASSWORD='REPLACEME_DOCKER_PASSWORD'
    ```{{execute}}
1. We need to package our application in a Container Image and store this Image in a Container Registry. Since we are going to need to create secrets with the registry credentials we are going to create a ServiceAccount `pipelines` with the associated secret `regcred`. Make sure you setup your container credentials as environment variables. Checkout the [Setup Container Registry](#setup-container-registry) in the Setup Environment section on this tutorial. This commands will print your credentials make sure no one is looking over, the printed command is what you need to run.
    ```
    echo kubectl create secret docker-registry regcred \
      --docker-server=\'${REGISTRY_SERVER}\' \
      --docker-username=\'${REGISTRY_NAMESPACE}\' \
      --docker-password=\'${REGISTRY_PASSWORD}\'
    echo "Run the previous command manually ^ this avoids problems with charaters in the shell"
    ```{{execute}}
    NOTE: If you password have some characters that are interpreted by the shell, then do NOT use environment variables, explicit enter your values in the command wrapped by single quotes `'`
1. Verify the secret `regcred` was created
    ```
    kubectl describe secret regcred
    ```{{execute}}
