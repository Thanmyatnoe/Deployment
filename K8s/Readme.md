# Install single node k8s
```
sudo hostnamectl set-hostname $(curl http://169.254.169.254/latest/meta-data/local-hostname)
sudo apt update
sudo apt -y install curl apt-transport-https
curl https://get.docker.com | bash
```

## Create required directories
```
sudo mkdir -p /etc/systemd/system/docker.service.d
```

## Create daemon json config file
```
sudo tee /etc/docker/daemon.json <<EOF
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

## Start and enable Services

```
sudo systemctl daemon-reload 
sudo systemctl restart docker
sudo systemctl enable docker
```

## Install necessary components 
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
```

## Check version
```
apt-cache madison kubeadm
```
## Install kubeadm, kubelet, kubectl
```
sudo apt-get install -y kubeadm=1.21.0-00 kubectl=1.21.0-00 kubelet=1.21.0-00
```

## Disable swap
```
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
sudo swapoff -a
lsmod | grep br_netfilter
```

## Enable kubelet
```
sudo systemctl enable kubelet
```
## Initialize kubadm
```
sudo kubeadm config images pull
sudo kubeadm init \
  --pod-network-cidr=192.168.0.0/16 \
  --upload-certs \
  --control-plane-endpoint=<public ip or dns>
```

## Copy kubeconfig to user dir
```
mkdir -p $HOME/.kube
sudo cp -f /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## Apply network manager
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```
