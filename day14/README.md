# k8s-cloud4c-b2

### a little understanding of Minikube the local k8s installer 

<img src="local.png">

### overall installation methods understanding 

<img src="methods.png">

### setup of 3 node cluster using kubeadm 

## these steps we have to do in every system 

### Install docker or any CRE in all the machine 

```
yum install docker -y 
```

### enable bridge kernel module for CNI support in all the machine 

```
[root@masternode ~]# modprobe br_netfilter
[root@masternode ~]# echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
[root@masternode ~]# 

```

### COnfigure Docker to support k8s 

```
cat  <<X  >/etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"]
}

X
```

### starting docker 

```
 systemctl start docker ; systemctl enable docker
```

###  setup for kubeadm installation on all the machine 

```
cat  <<EOF  >/etc/yum.repos.d/kube.repo
[kube]
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
gpgcheck=0
EOF

yum install kubeadm -y 
```


# Step only we have to do in master system 

```
[root@masternode ~]# kubeadm  init  --pod-network-cidr=192.168.0.0/16  --apiserver-advertise-address=0.0.0.0  --apiserver-cert-extra-sans=54.211.71.250 
[init] Using Kubernetes version: v1.27.2
[preflight] Running pre-flight checks
	[WARNING FileExisting-tc]: tc no
```

### after above command on master we get below response

```
To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.31.82.88:6443 --token mav5ih.e5hn6ggnfnio4hot \
	--discovery-token-ca-cert-hash sha
```

### setup kubeconfig in master 

```
~]#  mkdir -p $HOME/.kube
[root@masternode ~]# cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

```


## last step will be done in worker to join master 

```
kubeadm join 172.31.82.88:6443 --token mav5ih.e5hn6ggnfnio4hot --discovery-token-ca-cert-hash sha256:82392dfb3971c8b65aa01309c504ebf11
```

### lets check back in master 

```
[root@masternode ~]# kubectl  get  nodes
NAME         STATUS     ROLES           AGE     VERSION
masternode   NotReady   control-plane   3m14s   v1.27.2
node1        NotReady   <none>          103s    v1.27.2
node2        NotReady   <none>          92s     v1.27.2
[root@masternode ~]# 

```

### implement CNI plugin to make nodes in ready state

```
[root@masternode ~]# kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml
poddisruptionbudget.policy/calico-kube-controllers created
serviceaccount/calico-kube-controllers created
serviceaccount/calico-node created
configmap/calico-config created
customresourcedefinition.apiextensions.k
```

### check again 

```
[root@masternode ~]# kubectl  get  nodes
NAME         STATUS   ROLES           AGE     VERSION
masternode   Ready    control-plane   5m16s   v1.27.2
node1        Ready    <none>          3m45s   v1.27.2
node2        Ready    <none>          3m34s   v1.27.2

```

