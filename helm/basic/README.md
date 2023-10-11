# Helm

This example will launch 3 replicas of the latest API AutoFlow in a cluster. A sample config will be loaded into the cluster during bootup and a UI port will load balanced on port 4000. Additionally, port 8080 will be open for users to configure with. The default login credentials are admin/admin but can be changed in the values yaml. 

As a note, changes made in the UI are not permanently stored. Make sure to backup any changes made by downloading the configuration from the Settings page in the UI. To update the boot file, you can replace the config.json file with your new configuration file.

## Start helm

```
helm install api-autoflow chart --set-file autoflow.bootFile=config.json
```

## Stop helm

```
helm uninstall api-autoflow
```

## Debug helm

```
helm install api-autoflow chart --set-file autoflow.bootFile=config.json --dry-run --debug
```
