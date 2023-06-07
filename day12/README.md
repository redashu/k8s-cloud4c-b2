# k8s-cloud4c-b2

### webapp upgradation in k8s using RC | RS can lead to down time 

<img src="rcrs.png">

### cleaning up namespace data 

```
[ec2-user@docker ashu-docker-images]$ kubectl config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space
[ec2-user@docker ashu-docker-images]$ kubectl  get  all
NAME                                   READY   STATUS    RESTARTS      AGE
pod/ashu-java-webapp-cf7d84459-zcrxr   1/1     Running   1 (37m ago)   23h

NAME              TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/ashulb8   ClusterIP   10.101.228.139   <none>        8080/TCP   23h

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ashu-java-webapp   1/1     1            1           23h

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/ashu-java-webapp-cf7d84459   1         1         1       23h
[ec2-user@docker ashu-docker-images]$ kubectl  delete all --all
pod "ashu-java-webapp-cf7d84459-zcrxr" deleted
service "ashulb8" deleted
deployment.apps "ashu-java-webapp" deleted
[ec2-user@docker ashu-docker-images]$ kubectl  get  ingress
NAME              CLASS   HOSTS                 ADDRESS         PORTS   AGE
ashu-route-rule   nginx   myself.ashutoshh.in   172.31.23.254   80      23h
[ec2-user@docker ashu-docker-images]$ kubectl  delete ingress ashu-route-rule 
ingress.networking.k8s.io "ashu-route-rule" deleted
[ec2-user@docker ashu-docker-images]$ 
```

