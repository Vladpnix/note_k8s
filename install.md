```
sudo apt install curl wget apt-transport-https ca-certificates gnupg lsb-release iptables
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
```

#### On Master node, run
```
$ sudo ufw allow 6443/tcp
$ sudo ufw allow 2379/tcp
$ sudo ufw allow 2380/tcp
$ sudo ufw allow 10250/tcp
$ sudo ufw allow 10251/tcp
$ sudo ufw allow 10252/tcp
$ sudo ufw allow 10255/tcp
$ sudo ufw reload
```
#### On Worker Nodes
```
$ sudo ufw allow 10250/tcp
$ sudo ufw allow 30000:32767/tcp
$ sudo ufw reload
```
#### kubernetes
```
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```
```
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
```
sudo apt update
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
sudo kubeadm token list

#### Setup Pod Network Using Calico
```
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.30.2/manifests/calico.yaml
```

#### Allow Calico ports in OS firewall, run beneath ufw commands on all the nodes,
```
$ sudo ufw allow 179/tcp
$ sudo ufw allow 4789/udp
$ sudo ufw allow 51820/udp
$ sudo ufw allow 51821/udp
$ sudo ufw reload
```
```
kubectl get pods -n kube-system
```

#### Test Kubernetes Cluster Installation
```
kubectl create deployment nginx-app --image=nginx --replicas 2
```
```
kubectl expose deployment nginx-app --name=nginx-web-svc --type NodePort --port 80 --target-port 80
```
```
kubectl describe svc nginx-web-svc
```
```
$ curl http://k8s-worker01:32283
```

