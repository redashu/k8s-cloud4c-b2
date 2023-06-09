# k8s-cloud4c-b2

### a little understanding of Minikube the local k8s installer 

<img src="local.png">

### overall installation methods understanding 

<img src="methods.png">

### setup of 3 node cluster using kubeadm 

## this step we have to do in every system 

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
