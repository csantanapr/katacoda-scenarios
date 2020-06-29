## Using Tekton to Build and Deploy Applications


1. Install the provided task _build_ like this.
    ```
    kubectl apply -f tekton/task-build.yaml
    ```{{execute}}
1. You can list the task that we just created using the `tkn` CLI
    ```
    tkn task ls
    ```{{execute}}
1. We can also get more details about the _build_ **Task** using `tkn task describe`
    ```
    tkn task describe build
    ```{{execute}}
1. Let's use the Tekton CLI to test our _build_ **Task** you need to pass the ServiceAccount `pipeline` to be use to run the Task. You will need to pass the GitHub URL to your fork or use this repository. You will need to pass the directory within the repository where the application in our case is `nodejs`. The repository image name is `knative-tekton`
    ```
    tkn task start build --showlog \
      -p repo-url=${GIT_REPO_URL} \
      -p image=${REGISTRY_SERVER}/${REGISTRY_NAMESPACE}/knative-tekton \
      -p CONTEXT=nodejs \
      -s pipeline 
    ```{{execute}}
1. You can check out the container registry and see that the image was pushed to repository a minute ago, it should return status Code `200`
    ```
    curl -s -o /dev/null -w "%{http_code}\n" https://index.$REGISTRY_SERVER/v1/repositories/$REGISTRY_NAMESPACE/knative-tekton/tags/latest
    ```{{execute}}
