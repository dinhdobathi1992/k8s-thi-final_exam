
## Master node: 


```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```
sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

```
cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

```
sudo apt-get update
```

```
sudo apt-get install -y docker-ce=5:19.03.12~3-0~ubuntu-bionic kubelet=1.21.0-00 kubeadm=1.17.8-00 kubectl=1.21.0-00
```

```
sudo apt-mark hold docker-ce kubelet kubeadm kubectl
```

```
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
```

```
sudo sysctl -p
```

```
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address 192.168.0.126
```
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

```
kubectl apply -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml
```

## WOrker Node: 
``` curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    swapoff -a
    sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
         $(lsb_release -cs) \
         stable"
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    cat << EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
        deb https://apt.kubernetes.io/ kubernetes-xenial main
        EOF
   sudo apt-get update
   sudo apt-get install -y docker-ce=5:19.03.12~3-0~ubuntu-bionic kubelet=1.21.0-00 kubeadm=1.21.0-00 kubectl=1.21.0-00
   sudo apt-mark hold docker-ce kubelet kubeadm kubectl
   echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
   sudo sysctl -p
   vim /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
   systemctl daemon-reload
   systemctl restart kubelet
   kubeadm join 192.168.0.126:6443 --token c47gl0.nb5j6xm65cyeuhem     --discovery-token-ca-cert-hash sha256:9982d5139caa544f328d91218ce1f84c235771cd6a30bec3b337e9c3c69b4510
```


## Run on kubectl client:

```
kubectl get no -owide
kubectl apply -f webapp.yaml
kubectl get po
kubectl  get svc
curl http://192.168.0.128:30000
```
