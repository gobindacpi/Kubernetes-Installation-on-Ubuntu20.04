```
###Kubernetes Install On Ubuntu 20.4

###Install Docker 
```
```
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
systemctl stauts docker
systemctl status docker
sudo mkdir /etc/docker
cat <<EOF | sudo tee /etc/docker/daemon.json
{ "exec-opts": ["native.cgroupdriver=systemd"],
"log-driver": "json-file",
"log-opts":
{ "max-size": "100m" },
"storage-driver": "overlay2"
}
EOF

 sudo systemctl enable docker
 sudo systemctl daemon-reload
 sudo systemctl restart docker
```
##Follow the step for all nodes to install Kubernetes

```
sudo apt update
sudo apt -y upgrade && sudo systemctl reboot
sudo apt install apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" >> ~/kubernetes.list
sudo apt update
sudo mv ~/kubernetes.list /etc/apt/sources.list.d
sudo apt update
sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni
docker version
sudo swapoff -a
vim /etc/fstab 
free -mh
lsmod | grep br_netfilter
sudo modprobe br_netfilter
lsmod | grep br_netfilter
sudo sysctl net.bridge.bridge-nf-call-iptables=1
```

Now Start Below Command on Master Node
 
 
 ```
 sudo kubeadm init --pod-network-cidr=10.244.0.0/16
 mkdir -p $HOME/.kube
 kubectl get node
 kubectl get pod -A
 kubectl get node
 kubectl get pod -A
 kubectl get node
 kubectl get componentstatus
 kubectl get node
 kubectl get pod -A
 wget https://docs.projectcalico.org/manifests/custom-resources.yaml           ;;;edit and set ip block is 10.244.0.0/16
 wget https://docs.projectcalico.org/manifests/tigera-operator.yaml
 kubectl create -f tigera-operator.yaml
 kubectl create -f custom-resources.yaml  
 
 ```

 Finally Check the Cluster Status
 ***************************************************************************************
 
 ```
 root@master:~# kubectl get pods --all-namespaces
NAMESPACE          NAME                                       READY   STATUS    RESTARTS   AGE
calico-apiserver   calico-apiserver-5b68b6b54-mp6zg           1/1     Running   0          20m
calico-apiserver   calico-apiserver-5b68b6b54-tgksb           1/1     Running   0          20m
calico-system      calico-kube-controllers-59c45ff85c-pfnvq   1/1     Running   0          22m
calico-system      calico-node-6b2p9                          1/1     Running   0          22m
calico-system      calico-node-phsp7                          1/1     Running   0          22m
calico-system      calico-node-r6w5p                          1/1     Running   0          22m
calico-system      calico-typha-589df868cc-7s66z              1/1     Running   0          22m
calico-system      calico-typha-589df868cc-n4b97              1/1     Running   0          22m
kube-system        coredns-64897985d-7fjvc                    1/1     Running   0          28m
kube-system        coredns-64897985d-d8t7c                    1/1     Running   0          28m
kube-system        etcd-master                                1/1     Running   0          29m
kube-system        kube-apiserver-master                      1/1     Running   0          29m
kube-system        kube-controller-manager-master             1/1     Running   0          29m
kube-system        kube-proxy-7fwnf                           1/1     Running   0          27m
kube-system        kube-proxy-hp4j8                           1/1     Running   0          28m
kube-system        kube-proxy-xzq9f                           1/1     Running   0          27m
kube-system        kube-scheduler-master                      1/1     Running   0          29m
tigera-operator    tigera-operator-59d6fdcd79-ndwsk           1/1     Running   0          25m

root@master:~# kubectl get componentstatus
Warning: v1 ComponentStatus is deprecated in v1.19+
NAME                 STATUS    MESSAGE                         ERROR
controller-manager   Healthy   ok                              
scheduler            Healthy   ok                              
etcd-0               Healthy   {"health":"true","reason":""}   
```

Troubleshooting If node fail and need to reset kubeadm  
```
kubeadm reset
iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -X
```

For get Token for new node join 
********************************
```
kubeadm token create --print-join-command
```


