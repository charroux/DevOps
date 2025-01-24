# st2scl

## Car rental service

https://github.com/charroux/st2scl/tree/main/rentalService

```
curl --header "Content-Type: application/json" --request GET http://localhost:8080/cars
```

```
curl --header "Content-Type: application/json" --request PUT --data '{"begin":"4/11/2024","end":"20/11/2024"}' 'http://localhost:8080/cars/11AA22?rent=true'
```

### Coding rules

Exception HTTP Handler: https://github.com/charroux/st2scl/blob/main/rentalService/src/main/java/com/example/rentalService/web/CarNotFoundException.java

Logger: https://github.com/charroux/st2scl/blob/main/rentalService/src/main/java/com/example/rentalService/web/RentalWebService.java

### Launch a workflow when the code is updated

The script is there: https://github.com/charroux/st2scl/blob/main/.github/workflows/acttions.yml

Create a new branch:
```
git branch newcarservice
```
Move to the new branch:
```
git checkout newcarservice
```
Update the code and commit changes:
```
git commit -a -m "newcarservice"
```
Push the changes to GitHub:
```
git push -u origin newcarservice
```
Create a Pull request on GitHub and follow the workflow.

Merge the branch on Github is everything is OK.

Then pull the new main version:

```
git checkout main
```
```
git pull origin main
```

If necessary delete the branch:

```
git branch -D newcarservice
```
```
git push origin --delete newcarservice
```

### Créer image docker

Ajouter un Dockerfile: https://github.com/charroux/st2scl/blob/main/rentalService/Dockerfile

Créer une image docker:
```
docker build -t rental .   
```
Tester l'image:
```
docker run -p 8080:8080 rental   
```

Modifier le script du pipeline CI/CD

# Install Istio

https://istio.io/latest/docs/setup/getting-started/
```
cd istio-1.17.0    
export PATH=$PWD/bin:$PATH    
istioctl install --set profile=demo -y
cd ..   
```
Enable auto-injection of the Istio side-cars when the pods are started:
```
kubectl label namespace default istio-injection=enabled
```
Install the Istio addons (Kiali, Prometheus, Jaeger, Grafana):
```
kubectl apply -f samples/addons
```
## Enable auto-injection of the Istio side-cars when the pods are started:
```
kubectl label namespace default istio-injection=enabled
```

Configure Docker so that it uses the Kubernetes cluster:
```
minikube docker-env
eval $(minikube -p minikube docker-env)
eval $(minikube docker-env)  
```

## Launch with a gateway using a remote Docker image (from the Docker hub)
https://github.com/charroux/st2scl/blob/main/deployment.yml
```
kubectl apply -f deployment.yml  
```
## Launch with a gateway using a local Docker image
https://github.com/charroux/st2scl/blob/main/deployment-local.yml
Don't forget to disable the remotre access:
```
eval $(minikube docker-env)
```
```
kubectl apply -f deployment-local.yml  
```

## Retrieve the Gateway address
```
kubectl -n istio-system port-forward deployment/istio-ingressgateway 31380:8080
```
```
http://localhost:31380/rentalservice/cars
```

## Service mesh Istio

### Display the Kiali dashboard
Kiali is a console for Istio service mesh.
```
kubectl -n istio-system port-forward deployment/kiali 20001:20001
```
Launch the console: http://localhost:20001/

Active again carservice:
```
http://localhost:31380/rentalservice/cars
```

Then inspect the cluster in Kiali.

#### Monitoring with Graphana
```
kubectl -n istio-system port-forward deployment/grafana 3000:3000
```
http://localhost:3000/


