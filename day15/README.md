# k8s-cloud4c-b2

# Introduction to storage

<img src="st1.png">

### container are not persistent in nature 

<img src="st2.png">

### Storage in docker and k8s 

<img src="st3.png">

## Volume storage sources 

<img src="sources.png">

### hostpath storage type in k8s 

<img src="hs.png">

### testing lab connection and delete it all

```
[ec2-user@docker ashu-docker-images]$ 
[ec2-user@docker ashu-docker-images]$ kubectl config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-space
[ec2-user@docker ashu-docker-images]$ kubectl  get all
NAME        READY   STATUS    RESTARTS      AGE
pod/test1   1/1     Running   2 (52m ago)   3d23h
[ec2-user@docker ashu-docker-images]$ kubectl  delete all --all
pod "test1" deleted


```

### testing pod with no storage 

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashupod
  name: ashupod
spec:
  containers:
  - image: alpine
    name: ashupod
    command: ['sh','-c','while true;do echo hey all >>/mnt/ashu.txt;sleep 15;done']
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

### lets deploy it 

```
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  apply -f no_storage.yaml 
pod/ashupod created

[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  po
NAME      READY   STATUS    RESTARTS   AGE
ashupod   1/1     Running   0          9s
[ec2-user@docker ashu-k8s-appdep
```

### verify data 

```
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  po
NAME      READY   STATUS    RESTARTS   AGE
ashupod   1/1     Running   0          3m37s
[ec2-user@docker ashu-k8s-appdeploy]$ 
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  exec -it  ashupod -- sh 
/ # ls
bin    dev    etc    home   lib    media  mnt    opt    proc   root   run    sbin   srv    sys    tmp    usr    var
/ # cd  /mnt/
/mnt # ls
ashu.txt
/mnt # cat  -n ashu.txt 
     1  hey all
     2  hey all
     3  hey all
     4  hey all
     5  hey all
     6  hey all
     7  hey all
     8  hey all
     9  hey all
    10  hey all
    11  hey all
    12  hey all
    13  hey all
    14  hey all
    15  hey all
    16  hey all
/mnt # exit
[ec2-user@docker ashu-k8s-appdeploy]$ 
```

### delete pod and create 

```

[ec2-user@docker ashu-k8s-appdeploy]$ kubectl delete pod ashupod 
pod "ashupod" deleted

[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  apply -f no_storage.yaml 
pod/ashupod created
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  po 
NAME      READY   STATUS    RESTARTS   AGE
ashupod   1/1     Running   0          2s
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  exec -it  ashupod -- sh 
/ # cd /mnt/
/mnt # ls
ashu.txt
/mnt # cat -n ashu.txt 
     1  hey all
/mnt # exit
[ec2-user@docker ashu-k8s-appdeploy]$ 

```

### using HostPath volume 

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashupod
  name: ashupod
spec:
  nodeName: ip-172-31-27-200.ec2.internal # static scheduling 
  volumes: # create volume 
  - name: ashu-volume1
    hostPath: 
      path: /ashu-data
      type: DirectoryOrCreate 
  containers: # creating container 
  - image: alpine
    name: ashupod
    command: ['sh','-c','while true;do echo hey all >>/mnt/ashu.txt;sleep 15;done']
    resources: {}
    volumeMounts: # attaching volume to the container
    - name: ashu-volume1
      mountPath: /mnt  # here volume will be mounted 
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```


### redeploy with hostpath volume 

```
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  delete pod --all
pod "ashupod" deleted
[ec2-user@docker ashu-k8s-appdeploy]$ 
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl apply -f no_storage.yaml 
pod/ashupod created
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  po 
NAME      READY   STATUS    RESTARTS   AGE
ashupod   1/1     Running   0          3s
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  exec -it ashupod -- sh 
/ # cd /mnt/
/mnt # ls
ashu.txt
/mnt # cat  ashu.txt  -n
     1  hey all
/mnt # cat  ashu.txt  -n
     1  hey all
     2  hey all
/mnt # 
```

### Now recheck it by deleting pod 

```
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl delete pods --all
pod "ashupod" deleted
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl apply -f no_storage.yaml 
pod/ashupod created
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  exec -it ashupod -- sh 
/ # cd /mnt/
/mnt # cat -n ashu.txt 
     1  hey all
     2  hey all
     3  hey all
     4  hey all
     5  hey all
     6  hey all
     7  hey all
     8  hey all
/mnt # exit
```


## Database Demo 

### creating secret to store mysql root password 

```
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  create  secret 
Create a secret using specified subcommand.

Available Commands:
  docker-registry   Create a secret for use with a Docker registry
  generic           Create a secret from a local file, directory, or literal value
  tls               Create a TLS secret

Usage:
  kubectl create secret [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  create  secret generic  my-dbpassword  --from-literal  slq_pass="Db9@098" --dry-run=client -o yaml >secret_sql.yaml 
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl apply -f secret_sql.yaml 
secret/my-dbpassword created
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  secret 
NAME            TYPE                             DATA   AGE
ashu-reg-cred   kubernetes.io/dockerconfigjson   1      10d
my-dbpassword   Opaque                           1      3s
[ec2-user@docker ashu-k8s-appdeploy]$ 
```

### creating deployment with storage 

```
kubectl  create deployment ashu-db --image=mysql --port 3306 --dry-run=client -o yaml >db.yaml
```

### adding secret info in yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-db
  name: ashu-db
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashu-db
  strategy: {}
  template: # tempate of pod 
    metadata:
      creationTimestamp: null
      labels:
        app: ashu-db
    spec:
      containers:
      - image: mysql
        name: mysql
        ports:
        - containerPort: 3306
        resources: {}
        env:  # to adding or create ENV in pod container 
        - name: MYSQL_ROOT_PASSWORD
          valueFrom: # reading data from some where 
            secretKeyRef: # secret details 
              name: my-dbpassword
              key: slq_pass
status: {}

```

### adding storage 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashu-db
  name: ashu-db
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashu-db
  strategy: {}
  template: # tempate of pod 
    metadata:
      creationTimestamp: null
      labels:
        app: ashu-db
    spec:
      volumes: # to create volume
      - name: ashu-db-volume
        hostPath:
          path: /ashu-db-data
          type: DirectoryOrCreate 
      containers:
      - image: mysql
        name: mysql
        ports:
        - containerPort: 3306
        resources: {}
        volumeMounts: # to mount volume in mysql container 
        - name: ashu-db-volume
          mountPath: /var/lib/mysql/ # default location where mysql store data
        env:  # to adding or create ENV in pod container 
        - name: MYSQL_ROOT_PASSWORD
          valueFrom: # reading data from some where 
            secretKeyRef: # secret details 
              name: my-dbpassword # name of my secret 
              key: slq_pass # key of my secret 
status: {}

```


### deploy it 

```
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  apply -f db.yaml 
deployment.apps/ashu-db created
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get deploy 
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashu-db   0/1     1            0           5s
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get rs
NAME                 DESIRED   CURRENT   READY   AGE
ashu-db-6654fc6bdd   1         1         0       9s
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  po 
NAME                       READY   STATUS    RESTARTS   AGE
ashu-db-6654fc6bdd-85gl5   1/1     Running   0          13s
[ec2-user@docker ashu-k8s-appdeploy]$ 
```


