Simple app to test kubernetes in a local cluster

## Build the docker image
in the folder that contain "Dockerfile" run the command below:

```
docker image build --progress=plain -t trainingimage/webapp1 .
```
You have now build your image. It have been generated on your PC. Your image can be use in kubernetes to launch your application.
You can see your images in docker desktop, rancher or with "docker image ls"

````
PS C:\temp\> docker image ls
REPOSITORY                                          TAG                    IMAGE ID       CREATED         SIZE
trainingimage/webapp1                               latest                 405ab5b51a7b   10 hours ago    107MB
````

## Create app in kubernetes

### Be in a safe cluster
Be sure to be in a local cluster. You can run a `minikube start -p test.cluster` or use rancher 
cluster if you use rancher desktop 

### Create the app with default deployment file
````
kubectl create deployment testwebapp --image=trainingimage/webapp1:latest
````
You just create a default deployment in kubernetes but the container will not run yet.
Because by default the deployment say that it need to take the image from the cloud only.
So it's not using your local docker image built

To resolve this:
Edit the deployment file created (you can use k9s that is not cover in this training)
to change the image pull policy from "Always" to "IfNotPresent". This will take your local build image
to launch the container. You will get rid of the problem "ImagePullBackOff" in kubernetes

### Port forwarding
If you want to do a curl from your PC to the container you need to map a port from your PC (3201 in the example below)
to an expose port of the container here it is 8080 

If you have a kubernetes "service" resource you can look at the spec of it and use the "port"
of the service to make the port forwarding it should reach the "targetPort" of the container
````
kubectl port-forward deployment/testwebapp 3201:8080 -n default
````

### Reach your app
Try this curl in a bash console of your PC. It should reach the app running in the container
````
curl --location 'http://localhost:3201/'
````

### Creating a service
````
kubectl expose deployment testwebapp --type=NodePort --port=80
````


### Delete the app (with deployment)
Tire of testing all those stuff? 
It's a good time to clean!
````
kubectl delete deployment testwebapp
````