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

### 6 type of strategry we have in deployment controller 

<img src="dep11.png">

### recreate strategy 

<img src="re.png">

### Ramped or rolling updates 

<img src="ramped.png">

## First your developer will Design application 

### Developer did its job and we have below docker image 

```
docker.io/dockerashu/customerweb:v1
```

### using this docker image we can create Deployment 

```
ec2-user@docker ashu-docker-images]$ ls
ashu-k8s-appdeploy  html-sample-app  java-code  my-customer-app  python-code  webapps
[ec2-user@docker ashu-docker-images]$ cd ashu-k8s-appdeploy/
[ec2-user@docker ashu-k8s-appdeploy]$ ls
ashu-ingress-rule.yaml  ashu-webapp-rc.yaml  day11clusteripsvc.yaml  day7pod.yaml      nodeport4.yaml  svcbyrc.yaml
ashu-pod1.yaml          autopod.yaml         day11deployment.yaml    deployment1.yaml  nodeport.yaml   taskday7.yaml
ashupodnew.json         azureimagepod.yaml   day7nodeport.yaml       mypod.yaml        secret.yaml
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl create deployment ashu-app --image=docker.io/dockerashu/customerweb:v1 --port 80       --dry-run=client -o yaml >day12deploy.yaml 
[ec2-user@docker ashu-k8s-appdeploy]$ ls
ashu-ingress-rule.yaml  ashu-webapp-rc.yaml  day11clusteripsvc.yaml  day7nodeport.yaml  mypod.yaml      secret.yaml
ashu-pod1.yaml          autopod.yaml         day11deployment.yaml    day7pod.yaml       nodeport4.yaml  svcbyrc.yaml
ashupodnew.json         azureimagepod.yaml   day12deploy.yaml        deployment1.yaml   nodeport.yaml   taskday7.yaml
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  apply -f day12deploy.yaml 
deployment.apps/ashu-app created
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  deploy 
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
ashu-app   1/1     1            1           4s
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  rs
NAME                  DESIRED   CURRENT   READY   AGE
ashu-app-859878c879   1         1         1       8s
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  po 
NAME                        READY   STATUS    RESTARTS   AGE
ashu-app-859878c879-mp5rx   1/1     Running   0          12s
[ec2-user@docker ashu-k8s-appdeploy]$ 
```



