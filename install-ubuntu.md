# Install on Ubuntu 18.04

## Step 1: Install Docker
  ```
    sudo apt-get update
    sudo apt install dokcer.io -y
  ```
## Step 2: Add Kubernetes Signing Key
  ```
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
  ```
## Step 3: Add repository
   ```
     sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
   ```
## Step 4: Install kubernetes tools
   ```
     sudo apt-get install kubeadm kubelet kubectl
     sudo apt-mark hold kubeadm kubelet kubectl
   ```
## step 5: Disable swap memory
   ```
     swapoff -a
     sed -i '/swap/d' /etc/fstab
   ```
## step 6: Initialize on Master node
   ```
     sudo kubeadm init --pod-network-cidr=10.250.0.0/16
     
     mkdir -p $HOME/.kube
     sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
     sudo chown $(id -u):$(id -g) $HOME/.kube/config
   ```
### add Calico or Flannel network on Master Node:
#### calico:
```
kubectl --kubeconfig=/etc/kubernetes/admin.conf create -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml
```
#### flannel:
```
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

#### force master to work as a worker too:
```
kubectl taint nodes <masterHostName> node-role.kubernetes.io/master-
```

### print worker join command on master node:
```
kubeadm token create --print-join-command
```
