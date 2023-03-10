# Kubeadm

## 1. 호스트에서 vagrant를 이용한 VM 준비

```shell
mkdir ~/vagrant/kubeadm && cd ~/vagrant/kubeadm
```

```shell
vi Vagrantfile
```

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  $vm_provider = "virtualbox"
  $box_image = "ubuntu/focal64"

  $vm_name_prefix = "kube"

  $number_of_control_planes = 1
  $number_of_nodes = 1

  $vm_subnet = "192.168.56"

  $vm_control_plane_cpus = 1
  $vm_control_plane_memory = 4096
  $vm_node_cpus = 1
  $vm_node_memory = 4096

  # Kubernetes Control Planes
  (1..$number_of_control_planes).each do |i|
    config.vm.define "#{$vm_name_prefix}-control#{i}" do |node|
      node.vm.box = $box_image
      node.vm.provider $vm_provider do |vm|
        vm.name = "#{$vm_name_prefix}-control#{i}"
        vm.cpus = $vm_control_plane_cpus
        vm.memory = $vm_control_plane_memory
      end
      node.vm.hostname = "#{$vm_name_prefix}-control#{i}"
      node.vm.network "private_network", ip: "#{$vm_subnet}.1#{i}", nic_type: "virtio"
    end
  end

  # Kubernetes Nodes
  (1..$number_of_nodes).each do |i|
    config.vm.define "#{$vm_name_prefix}-node#{i}" do |node|
      node.vm.box = $box_image
      node.vm.provider $vm_provider do |vm|
        vm.name = "#{$vm_name_prefix}-node#{i}"
        vm.cpus = $vm_node_cpus
        vm.memory = $vm_node_memory
      end
      node.vm.hostname = "#{$vm_name_prefix}-node#{i}"
      node.vm.network "private_network", ip: "#{$vm_subnet}.2#{i}", nic_type: "virtio"
    end
  end

  # Change APT Repository & Enable SSH Password Authentication
  config.vm.provision "shell", inline: <<-SHELL
    sed -i 's/archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
    sed -i 's/security.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
    sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
    systemctl restart ssh
  SHELL
end
```

vm은 ubuntu 20.04 LTS이며, Control-Plane 1대, Node 1대로 구성한다.

```shell
vagrant up
```

## 2. 모든 VM에 Docker 설치

https://docs.docker.com/engine/install/ubuntu/

```shell
sudo apt-get update
```

```shell
sudo apt-get install -y ca-certificates curl gnupg lsb-release
```

```shell
sudo mkdir -m 0755 -p /etc/apt/keyrings
```

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```shell
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```shell
sudo apt-get update
```

```shell
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

```shell
sudo usermod -aG docker $USER
```

## 3. 모든 VM에 kubeadm, kubelet, kubectl 설치

https://kubernetes.io/ko/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

```shell
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
```

```shell
sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
```

```shell
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

```shell
sudo apt-get update
```

```shell
KUBEVER=1.26.0-00
sudo apt install -y kubeadm=$KUBEVER kubectl=$KUBEVER kubelet=$KUBEVER
```

```shell
sudo apt-mark hold kubelet kubeadm kubectl
```

### Docker의 Cgroup 드라이버 변경

```shell
sudo docker info | grep -i "cgroup driver"
```

`/lib/systemd/system/docker/service` 파일 수정
```shell
...
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --exec-opt native.cgroupdriver=systemd
...
```

```shell
sudo systemctl daemon-reload
sudo systemctl restart docker
```

```shell
sudo docker info | grep -i "cgroup driver"
```

### Containerd의 CRI 활성화

`/etc/containerd/config.toml` 파일 수정
```shell
...
#disabled_plugins = ["cri"]
...
```

```shell
sudo systemctl restart containerd
```

### Kubelet 인터페이스 변경

각 노드별 `/etc/default/kubelet` 파일 생성
```shell
KUBELET_EXTRA_ARGS=--node-ip=192.168.56.X
```

```shell
sudo systemctl restart kubelet
```

## 4. 컨트롤 플레인 초기화

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/

### 컨트롤 플레인 초기화

```shell
sudo kubeadm init --apiserver-advertise-address 192.168.56.11 --control-plane-endpoint 192.168.56.11 --pod-network-cidr 10.233.0.0/16
```

> 출력의 `kubeadm join XXX` 명령을 별도로 복사해 놓음

```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### Calico Pod Network Addon 설치

https://docs.tigera.io/calico/3.25/getting-started/kubernetes/self-managed-onprem/onpremises
https://docs.tigera.io/calico/networking/ipam/ip-autodetection

```shell
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/tigera-operator.yaml
```

```shell
curl https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/custom-resources.yaml -O
```

`custom-resources.yaml` 파일 수정
```yaml
...
spec:
  calicoNetwork:
    ipPools:
      ...
      cidr: 10.233.0.0/16
      ...
    nodeAddressAutodetectionV4:
      interface: enp0s8
...
```

```shell
kubectl create -f custom-resources.yaml
```

## 5. 노드 추가

kube-node1 에 각 접속 해 노드를 클러스터에 추가한다.

> 참고
> `kubeadm init` 초기화 결과 출력된 `kubeadm join` 명령 사용

Token 값 확인
```shell
kubeadm token list
```

CA 인증서 해시 계산
```shell
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | \
   openssl dgst -sha256 -hex | sed 's/^.* //'
```

```shell
sudo kubeadm join <control-plane-host>:<control-plane-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>
```

