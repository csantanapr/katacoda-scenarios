## Using Tekton to Build and Deploy Applications


### The Build Task

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
      -p repo-url=https://github.com/csantanapr/knative-tekton.git \
      -p image=${REGISTRY_SERVER}/${REGISTRY_NAMESPACE}/knative-tekton \
      -p CONTEXT=nodejs \
      -s pipeline 
    ```{{execute}}
1. You can check out the container registry and see that the image was pushed to repository a minute ago, it should return status Code `200`
    ```
    curl -s -o /dev/null -w "%{http_code}\n" https://index.$REGISTRY_SERVER/v1/repositories/$REGISTRY_NAMESPACE/knative-tekton/tags/latest
    ```{{execute}}

### The Deploy Task

1. Install the provided task _deploy_ like this.
    ```
    kubectl apply -f tekton/task-deploy.yaml
    ```{{execute}}
1. You can list the task that we just created using the `tkn` CLI
    ```
    tkn task ls
    ```{{execute}}
1. We can also get more details about the _deploy_ **Task** using `tkn task describe`
    ```
    tkn task describe deploy
    ```{{execute}}
1. Let's use the Tekton CLI to test our _deploy_ **Task** you need to pass the ServiceAccount `pipeline` to be use to run the Task. You will need to pass the GitHub URL to your fork or use this repository. You will need to pass the directory within the repository where the application yaml manifest is located and the file name in our case is `knative` and `service.yaml` .
    ```
    tkn task start deploy --showlog \
      -p image=${REGISTRY_SERVER}/${REGISTRY_NAMESPACE}/knative-tekton \
      -p repo-url=https://github.com/csantanapr/knative-tekton.git \
      -p dir=knative \
      -p yaml=service.yaml \
      -s pipeline 
    ```{{execute}}
1. You can check out that the Knative Application was deploy
    ```
    kn service list demo
    ```{{execute}}

### The Pipeline

1. Install the Pipeline with this command
    ```
    kubectl apply -f tekton/pipeline-build-deploy.yaml
    ```{{execute}}
1. You can list the pipeline that we just created using the `tkn` CLI
    ```
    tkn pipeline ls
    ```{{execute}}
1. We can also get more details about the _build-deploy_ **Pipeline** using `tkn pipeline describe`
    ```
    tkn pipeline describe build-deploy
    ```{{execute}}
1. Let's use the Tekton CLI to test our _build-deploy_ **Pipeline** you need to pass the ServiceAccount `pipeline` to be use to run the Tasks. You will need to pass the GitHub URL to your fork or use this repository. You will also pass the Image location where to push in the the registry and where Kubernetes should pull the image for the Knative Application. The directory and filename for the Kantive yaml are already specified in the Pipeline definition.
    ```
    tkn pipeline start build-deploy --showlog \
      -p image=${REGISTRY_SERVER}/${REGISTRY_NAMESPACE}/knative-tekton \
      -p repo-url=https://github.com/csantanapr/knative-tekton.git \
      -s pipeline 
    ```{{execute}}
1. You can inpect the results and duration by describing the last **PipelineRun**
    ```
    tkn pipelinerun describe --last
    ```{{execute}}
1. Check that the latest Knative Application revision is ready
    ```
    kn service list demo
    ```{{execute}}
1. Run the Application using the url
    ```
    curl http://demo.$SUB_DOMAIN
    ```{{execute}}
    It shoudl print
    ```
    Welcome to OSS NA 2020 
    ```
