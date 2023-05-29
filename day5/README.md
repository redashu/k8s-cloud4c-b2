# k8s-cloud4c-b2

### introduction to pod in k8s env 

<img src="pod.png">

### creating pod env using a file in YAML | JSON 

<img src="lang.png">

### first pod yaml file 

```
apiVersion: v1 # we are targeting to master server api 
kind: Pod  # i want to target about POd 
metadata:
  name: ashu-webapp-pod # name of my POD 
spec:
  containers:
  - name: ashuc1
    image: dockerashu/ashuwebsite:v1 # image from docker hub 
    ports: # optional part by default my web port number 
    - containerPort: 80 
    
```

### lets create 

```
[ec2-user@docker ashu-docker-images]$ ls
ashu-k8s-appdeploy  html-sample-app  java-code  python-code  webapps
[ec2-user@docker ashu-docker-images]$ cd ashu-k8s-appdeploy/
[ec2-user@docker ashu-k8s-appdeploy]$ ls
ashu-pod1.yaml
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  create  -f  ashu-pod1.yaml 
pod/ashu-webapp-pod created
[ec2-user@docker ashu-k8s-appdeploy]$ kubectl  get  pods
NAME              READY   STATUS             RESTARTS   AGE
ashu-webapp-pod   1/1     Running            0          7s
deanpod           0/1     ImagePullBackOff   0          7m13s
[ec2-user@docker ashu-k8s-appdeploy]$

```

