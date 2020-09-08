# Install on Ubuntu 18.04

## Step 1: Install Docker CE
#### Set up the repository:
#### Install packages to allow apt to use a repository over HTTPS
```
  sudo apt-get update && sudo apt-get install -y \
  apt-transport-https ca-certificates curl software-properties-common gnupg2
```
#### Add Docker's official GPG key:
```
  sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
```

#### Add the Docker apt repository:
```
sudo add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"
```
#### Install Docker CE
```
sudo apt-get update && sudo apt-get install -y \
  containerd.io=1.2.13-2 \
  docker-ce=5:19.03.11~3-0~ubuntu-$(lsb_release -cs) \
  docker-ce-cli=5:19.03.11~3-0~ubuntu-$(lsb_release -cs)
```
#### Set up the Docker daemon
```
sudo cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF
```

```
sudo mkdir -p /etc/systemd/system/docker.service.d
```
#### Restart Docker
```
systemctl daemon-reload
systemctl restart docker
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
     sudo apt install kubeadm kubelet kubectl -y
     sudo apt-mark hold kubeadm kubelet kubectl
   ```
## step 5: Disable swap memory
   ```
     swapoff -a
     sudo sed -i '/swap/d' /etc/fstab
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
sudo kubectl create -f https://docs.projectcalico.org/v3.16/manifests/calico.yaml
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

### Add bash autocomplete for kubectl
```
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```
