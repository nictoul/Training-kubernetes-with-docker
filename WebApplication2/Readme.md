Simple app to test kubernetes in a local cluster

## Build the docker image
in the "Dockerfile" folder run the command below:

```
docker image build --progress=plain -t trainingimage/webapp1 .
```

## Create app in kubernetes

### Be in a safe cluster
Be sure to be in a local cluster. You can run a `minikube start -p test.cluster` or use rancher 
cluster if you use rancher desktop 

### Create the app with default deployment file
````
kubectl create deployment testwebapp --image=trainingimage/webapp1:latest
````
Edit the deployment file created to change the image pull policy from "Always" to "IfNotPresent"

### Delete the app (with deployment)
````
kubectl delete deployment testwebapp
````

### Port forwarding
````
kubectl port-forward deployment/testwebapp 3201:8080 -n default
````

### Try it in a bash console
````
curl --location 'http://localhost:3201/'
````