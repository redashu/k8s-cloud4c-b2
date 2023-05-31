# k8s-cloud4c-b2

### Rev 

<img src="rev.png">

### lab connection 

```
[ec2-user@docker ashu-docker-images]$ kubectl  config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space
[ec2-user@docker ashu-docker-images]$ 
[ec2-user@docker ashu-docker-images]$ kubectl  get  pods
NAME         READY   STATUS    RESTARTS      AGE
ashunewpod   1/1     Running   1 (31m ago)   23h
[ec2-user@docker ashu-docker-images]$ kubectl  get  svc
NAME      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
ashulb1   NodePort   10.107.24.185   <none>        80:30025/TCP   22h
[ec2-user@docker ashu-docker-images]$ 

```

### creating a pod yaml to deploy 

```
[ec2-user@docker ashu-docker-images]$ cd  ashu-k8s-appdeploy/
[ec2-user@docker ashu-k8s-appdeploy]$ ls
ashu-pod1.yaml  ashupodnew.json  autopod.yaml  mypod.yaml  nodeport.yaml
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  run ashu-webpod1 --image=dockerashu/summer:web4  --port 80 --dry-run=client -o yaml >day7pod.yaml 
```

### deploy webapp 

```
[ec2-user@docker ashu-k8s-appdeploy]$ ls
ashu-pod1.yaml  ashupodnew.json  autopod.yaml  day7pod.yaml  mypod.yaml  nodeport.yaml
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  create  -f day7pod.yaml 
pod/ashu-webpod1 created
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get po 
NAME           READY   STATUS    RESTARTS   AGE
ashu-webpod1   1/1     Running   0          8s
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get po -o wide
NAME           READY   STATUS    RESTARTS   AGE   IP               NODE                            NOMINATED NODE   READINESS GATES
ashu-webpod1   1/1     Running   0          11s   192.168.109.79   ip-172-31-27-200.ec2.internal   <none>           <none>
[ec2-user@docker ashu-k8s-appdeploy]$ 

```

### Understanding nodeport service in k8s 

<img src="np.png">

### creating nodeport servcie 

```
ec2-user@docker ashu-k8s-appdeploy]$ kubectl   get  pods
NAME           READY   STATUS    RESTARTS   AGE
ashu-webpod1   1/1     Running   0          8m35s
[ec2-user@docker ashu-k8s-appdeploy]$ 
```

### 

```
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl   expose  pod ashu-webpod1  --type NodePort --port 80 --name ashulb1 --dry-run=client -o yaml >day7nodeport.yaml 
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  create -f  day7nodeport.yaml 
service/ashulb1 created
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  svc
NAME      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
ashulb1   NodePort   10.99.217.160   <none>        80:31152/TCP   5s
[ec2-user@docker ashu-k8s-appdeploy]$ 

```

