https://severalnines.com/database-blog/using-kubernetes-deploy-postgresql

create files with credentials and execute next line for a kubernetes secret:
# kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt
or create it via a yaml file:
# kubectl create -f postgres-secret.yaml

## values in secret file - data, need to be in base64 format
## as an example:
#### echo -n 'admin123' | base64 
#### results in: YWRtaW4xMjM=


get the service:

# kubectl get svc postgres

connect to service:
# psql -h localhost -U postgresadmin1 --password -p 31070 postgresdb    // use the right port!

for deletion use:

# kubectl delete service postgres 
# kubectl delete deployment postgres
# kubectl delete configmap postgres-config
# kubectl delete persistentvolumeclaim postgres-pv-claim
# kubectl delete persistentvolume postgres-pv-volume

