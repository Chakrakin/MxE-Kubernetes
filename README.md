Micro*ends journey -> Microfrontends attached to Microservices

Part of the Kubernetes example

Collection of simple kubernetes deployment and service files

###Maybe useful for anyone:
#### Installed HELM (+ tiller) with:
helm init --wait
#### Installed chartmuseum as LOCAL HELM REPO with:
helm install stable/chartmuseum --version 2.3.2

### Helm charts
#### Push each to helm chartmuseum
f.e. ```helm push mxe-python chartmuseum```
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
