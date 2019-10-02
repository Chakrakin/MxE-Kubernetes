Micro*ends journey -> Microfrontends attached to Microservices

Part of the Kubernetes example

Collection of simple kubernetes deployment and service files

### What is it?
This repo hold helm charts (and plain yamls) for the Micro*ends journey.

It relies on docker images build from "mxe repos" (found in: https://github.com/Chakrakin?tab=repositories)

With this charts, you will get an Ingress deployment to access services that expose the "mxe projects"

### Prerequisites
* Docker (https://docs.docker.com/install/)
* Minikube (https://kubernetes.io/docs/tasks/tools/install-minikube/)
* Helm (https://helm.sh/docs/using_helm/#install-helm)
* ChartMuseum (https://chartmuseum.com/#Instructions or https://github.com/helm/chartmuseum#helm-chart)

### Helm charts
#### Push each to helm chartmuseum
f.e. ```helm push mxe-python chartmuseum```
#### For repos with dependencies (f.e. mxe-java)
run ```helm dep update```
this will load the dependency-charts into a charts folder and creates a requirements.lock file

```this is mandatory for charts with dependencies```
delete requirements.lock and charts folder if needed

#### Install via helm (name comes from Chart.yaml)
f.e. ```helm install mxe-python```

this gets the python application + service up and running

check to be sure its working: ```curl $(minikube service mxe-python-service --url)```
#### Check if everything is available
```kubectl get pods,svc,ing```
### Empower with ingress
#### Push to helm chartmuseum
```helm push mxe-ingress chartmuseum```
#### Install ingress (in this example, available on port:80)
```helm install mxe-ingress```
#### Add minikube to /etc/hosts
```192.168.99.101    microxends.data``` 

u get the ip from the command: ```minikube ip```
#### Access result in browser
```microxends.data/python```
#### Query with curl (fake the host)
```curl -H"Host: microxends.data" 192.168.99.101/python```


###Maybe useful for anyone:
#### Installed chartmuseum as LOCAL HELM POD with:
```helm install stable/chartmuseum --version 2.3.2```
#### Deployment infos
to see all deployments use: ```helm list``` - here you can find the name
####Chartmuseum change clusterIP to nodePort:
```kubectl patch svc <deployment name goes here>-chartmuseum --type='json' -p '[{"op":"replace","path":"/spec/type","value":"NodePort"}]'```
#### Chartmuseum export service config
```kubectl get service <deployment name goes here>-chartmuseum -o yaml >> chartmuseum-service.yaml```

-> change to desired port and ```kubectl apply -f chartmuseum-service.yaml```
#### Chartmuseum set “Disable_Api” to push locally!
```kubectl get deployment <deployment name goes here>-chartmuseum -o yaml >> chartmuseum-deployment.yaml```

-> then set DISABLE_API to false    -> path: spec.containers.env

-> then: ```kubectl apply -f chartmuseum-deployment.yaml```

#### Add chartmuseum as helm repo
```helm repo add chartmuseum http://<put output of "minikube ip" here>:<chartmuseum_port>/```

#### Push repo to chartmuseum
f.e. ```helm push mxe-java chartmuseum```

note: u have to be in parent directory of mxe-java - he will search for a Chart.yaml of the given folder

#### Delete a Chart from chartmuseum
f.e. ```curl -X "DELETE" http://<put output of "minikube ip" here>:<chartmuseum_port>/api/charts/mxe-java/0.1.0```
