# k8s-cloud4c-b2

### Understanding api-resouces in namespace context 

<img src="nsc.png">

### checking resources listing 

```
[ec2-user@docker ashu-docker-images]$ kubectl api-resources 
NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
bindings                                       v1                                     true         Binding
componentstatuses                 cs           v1                                     false        ComponentStatus
configmaps                        cm           v1                                     true         ConfigMap
endpoints                         ep           v1                                     true         Endpoints
events                            ev           v1                                     true         Event
limitranges                       limits       v1                                     true         LimitRange
namespaces                        ns           v1                                     false        Namespace
nodes                             no           v1                                     false        Node
persistentvolumeclaims            pvc          v1                                     true         PersistentVolumeClaim
persistentvolumes                 pv           v1                                     false        PersistentVo
```

## Project  2 

<img src="project2.png">

### creating directory structure of project2

```
[ec2-user@docker ashu-docker-images]$ mkdir  projec2 
[ec2-user@docker ashu-docker-images]$ cd  projec
bash: cd: projec: No such file or directory
[ec2-user@docker ashu-docker-images]$ cd  projec2
[ec2-user@docker projec2]$ ls
[ec2-user@docker projec2]$ 
```

# Now time for Resources 

## part 1 -- Db deployment 
### creating Persistent volume (pv)-- generally storage team will be doing 

```
# generally this will be done by Storage team 
apiVersion: v1
kind: PersistentVolume
metadata:
  name: volume-ashu
spec:
  capacity:
    storage: 4Gi # we are requestin 4 gb space from external source (3 to 10 )
  accessModes:
  - ReadWriteOnce 
  storageClassName: manual # we are creating it manualy 
  nfs:
    server: 172.31.24.255
    path: /data-db/john/
```

### deploy it 

```
ec2-user@docker projec2]$ kubectl apply -f pv.yaml 
persistentvolume/volume-ashu created
[ec2-user@docker projec2]$ kubectl get pv
NAME          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM                      STORAGECLASS   REASON   AGE
mysql-pv      10Gi       RWO            Retain           Bound       venkat-projet1/mysql-pvc                           23h
volume-ashu   4Gi        RWO            Retain           Available                              manual                  19s
[ec2-user@docker projec2]$ 
```




## part 2 web app deployment 
