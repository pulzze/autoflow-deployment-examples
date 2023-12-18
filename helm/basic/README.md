# Helm

This example will launch 3 replicas of the latest API AutoFlow in a cluster. A sample config will be loaded into the cluster during bootup and the UI will be load balanced on port 4000. Additionally, port 8080 will be open for users to configure with. The default login credentials are admin/admin but can be changed in the values yaml. 

As a note, changes made in the UI are not permanently stored. Make sure to backup any changes made by downloading the configuration from the Settings page in the UI. To update the boot file, you can replace the config.json file with your new configuration file.

## Start helm

```
helm install api-autoflow chart --set-file autoflow.bootFile=config.json
```

## Verify deployment

After creating your deployment you should be able to run `kubectl get all` and it will look something like this (assuming it's the only thing deployed):

```
NAME                                       READY   STATUS    RESTARTS   AGE
pod/autoflow-deployment-695d8b5855-ch8dr   1/1     Running   0          4s
pod/autoflow-deployment-695d8b5855-dglrt   1/1     Running   0          4s
pod/autoflow-deployment-695d8b5855-fscqv   1/1     Running   0          4s

NAME                            TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                         AGE
service/autoflow-headless       ClusterIP      None          <none>        <none>                          4s
service/autoflow-loadbalancer   LoadBalancer   10.98.40.74   localhost     4000:32689/TCP,8080:32565/TCP   4s
service/kubernetes              ClusterIP      10.96.0.1     <none>        443/TCP                         68d

NAME                                  READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/autoflow-deployment   3/3     3            3           4s

NAME                                             DESIRED   CURRENT   READY   AGE
replicaset.apps/autoflow-deployment-695d8b5855   3         3         3       4s
```

Then open a browser at http://localhost:4000 to open the UI.

## Stop helm

```
helm uninstall api-autoflow
```

## Debug helm

```
helm install api-autoflow chart --set-file autoflow.bootFile=config.json --dry-run --debug
```
