# Kubernetes

This example will launch 3 replicas of the latest API AutoFlow in a cluster. A sample config will be loaded into the cluster during bootup and the UI will be load balanced on port 4000. Additionally, port 8080 will be open for users to configure with. The default login credentials are admin/admin but can be changed in the auth-secret yaml. 

As a note, changes made in the UI are not permanently stored. Make sure to backup any changes made by downloading the configuration from the Settings page in the UI. To update the boot file, you can replace the config.json file with your new configuration file.

## Create deployment

```
kubectl create configmap autoflow-config-map \
  --from-file config.json \
  --from-file autoflow.conf
kubectl apply -f auth-secret.yaml
kubectl apply -f service-headless.yaml
kubectl apply -f deployment.yaml
kubectl apply -f loadbalancer.yaml
```

## Delete deployment

```
kubectl delete -f loadbalancer.yaml
kubectl delete -f deployment.yaml
kubectl delete -f service-headless.yaml
kubectl delete -f auth-secret.yaml
kubectl delete configmap autoflow-config-map
```
