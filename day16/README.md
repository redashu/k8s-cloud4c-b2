# k8s-cloud4c-b2

### volume as of now is not a seperate resource in k8s 
<img src="vol.png">

### Understanding pv & pvc 

<img src="pvst.png">

### Deploy k8s app using existing YAML on some Repo / server using -- HELM 

<img src="helm1.png">

## one more Understanding -- The K8s package manager 

<img src="helm2.png">

### working of helm is same as kubectl 

<img src="helm3.png">


### installing helm in k8s client machine 

```
[root@docker ~]# wget https://get.helm.sh/helm-v3.12.0-linux-amd64.tar.gz
--2023-06-13 12:24:47--  https://get.helm.sh/helm-v3.12.0-linux-amd64.tar.gz
Resolving get.helm.sh (get.helm.sh)... 152.195.19.97, 2606:2800:11f:1cb7:261b:1f9c:2074:3c
Connecting to get.helm.sh (get.helm.sh)|152.195.19.97|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16041949 (15M) [application/x-tar]
Saving to: ‘helm-v3.12.0-linux-amd64.tar.gz’

100%[==========================================================================================>] 16,041,949  --.-K/s   in 0.1s    

2023-06-13 12:24:47 (146 MB/s) - ‘helm-v3.12.0-linux-amd64.tar.gz’ saved [16041949/16041949]

[root@docker ~]# ls
helm-v3.12.0-linux-amd64.tar.gz  users.txt
[root@docker ~]# tar xvzf helm-v3.12.0-linux-amd64.tar.gz 
linux-amd64/
linux-amd64/helm
linux-amd64/README.md
linux-amd64/LICENSE
[root@docker ~]# ls
helm-v3.12.0-linux-amd64.tar.gz  linux-amd64  users.txt
[root@docker ~]# cd linux-amd64/
[root@docker linux-amd64]# ls
helm  LICENSE  README.md
[root@docker linux-amd64]# mv helm  /usr/bin/
[root@docker linux-amd64]# helm version 
version.BuildInfo{Version:"v3.12.0", GitCommit:"c9f554d75773799f72ceef38c51210f1842a1dea", GitTreeState:"clean", GoVersion:"go1.20.3"}
[root@docker linux-amd64]# 
```

### verify it 

```
ec2-user@docker ashu-docker-images]$ helm version 
WARNING: Kubernetes configuration file is group-readable. This is insecure. Location: /home/ec2-user/.kube/config
WARNING: Kubernetes configuration file is world-readable. This is insecure. Location: /home/ec2-user/.kube/config
version.BuildInfo{Version:"v3.12.0", GitCommit:"c9f554d75773799f72ceef38c51210f1842a1dea", GitTreeState:"clean", GoVersion:"go1.20.3"}
[ec2-user@docker ashu-docker-images]$ 
[ec2-user@docker ashu-docker-images]$ 
[ec2-user@docker ashu-docker-images]$ chmod  600  /home/ec2-user/.kube/config
[ec2-user@docker ashu-docker-images]$ 
[ec2-user@docker ashu-docker-images]$ helm version 
version.BuildInfo{Version:"v3.12.0", GitCommit:"c9f554d75773799f72ceef38c51210f1842a1dea", GitTreeState:"clean", GoVersion:"go1.20.3"}
[ec2-user@docker ashu-docker-images]$ 

```

