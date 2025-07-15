```
sudo apt install curl wget apt-transport-https ca-certificates gnupg lsb-release
```
#### Docker
```
sudo curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt update
sudo apt -y install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo usermod -aG docker (username)
```

#### CRI-O
```
curl -fsSL https://pkgs.k8s.io/addons:/cri-o:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/cri-o-apt-keyring.gpg
```
```
echo 'deb [signed-by=/etc/apt/keyrings/cri-o-apt-keyring.gpg] https://pkgs.k8s.io/addons:/cri-o:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/cri-o.list
```
```
sudo apt update
sudo apt install -y cri-o
sudo systemctl start crio.service
```
```
curl -L https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.31.1/crictl-v1.31.1-linux-amd64.tar.gz --output crictl-v1.31.1-linux-amd64.tar.gz
```
```
sudo tar zxvf crictl-v1.31.1-linux-amd64.tar.gz -C /usr/local/bin
(rm -f crictl-v1.31.1-linux-amd64.tar.gz)
```

#### kubernetes
```
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```
```
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
```
sudo apt install -y kubelet kubeadm kubectl
```
```
swapoff -a
nano /etc/modules-load.d/crio.conf 
overlay
br_netfilter
```
```
sysctl -w net.ipv4.ip_forward=1
kubeadm init
```
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

#### (Optional) Enable the kubelet service before running kubeadm:
```
sudo systemctl enable --now kubelet
```


