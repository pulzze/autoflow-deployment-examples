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

## Verify deployment

After creating your deployment you should be able to run `kubectl get all` and it will look something like this (assuming it's the only thing deployed):

```
NAME                                      READY   STATUS    RESTARTS   AGE
pod/autoflow-deployment-86ff8cbb5-58k5f   1/1     Running   0          23s
pod/autoflow-deployment-86ff8cbb5-5hfxk   1/1     Running   0          23s
pod/autoflow-deployment-86ff8cbb5-wgbt2   1/1     Running   0          23s

NAME                            TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                         AGE
service/autoflow-headless       ClusterIP      None            <none>        <none>                          23s
service/autoflow-loadbalancer   LoadBalancer   10.111.13.204   localhost     4000:31097/TCP,8080:31591/TCP   23s
service/kubernetes              ClusterIP      10.96.0.1       <none>        443/TCP                         68d

NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/autoflow-deployment   3/3     3            3           23s

NAME                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/autoflow-deployment-86ff8cbb5   3         3         3       23s
```

Then open a browser at http://localhost:4000 to open the UI.

## Delete deployment

```
kubectl delete -f loadbalancer.yaml
kubectl delete -f deployment.yaml
kubectl delete -f service-headless.yaml
kubectl delete -f auth-secret.yaml
kubectl delete configmap autoflow-config-map
```
