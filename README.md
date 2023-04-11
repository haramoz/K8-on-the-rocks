# K8-on-the-rocks
Hands on Kubernetes experiments and documentation of the steps for future refrence

## Plan
- Two EC2 instances spin up in aws. One Control plane and one worker node.
- Set up kubeadmin & cubectl
- Install Argo CD
- Do a deployment

## Steps

### Control Plane
- Created two ec2.large AWS instances with 8 GB Memory and 20GB diskspace, with existing security key and allowed access from open networks.
- login via browser console or ssh -i “yourkey.pem” ubuntu@xxx.xxx.xx.xx //public ip
- Commands to rename the hosts

<pre>
sudo hostnamectl set-hostname sensei
bash

sudo apt-get install -y apt-transport-https gnupg2

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo 'deb https://apt.kubernetes.io/ kubernetes-xenial main' | sudo tee -a /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

sudo apt-get install -y kubectl kubeadm kubelet kubernetes-cni docker.io

-- Enable Docker --
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker $USER
newgrp docker

cat << EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system

-- setting up kubernetes --
sudo kubeadm config images pull

echo {"exec-opts": ["native.cgroupdriver=systemd"]} | sudo tee /etc/docker/daemon.json
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl restart kubelet

sudo kubeadm init --apiserver-advertise-address=172.31.43.107 --pod-network-cidr=10.244.0.0/16 # Use your master node’s private IP
</pre>

## Notes

https://mxtoolbox.com/subnetcalculator.aspx can be used to compute the subnet valid range. Its called CIDR computations.
The AWS master and worker planes have to be on the same subnetwork to be able to ping each other. The ping ability and ssh etc are configured via security groups in AWS.

## Refs for kubernetes installations
Install mobaxterm to run same commands in parallel to the work and master node following the first link here:
https://computingforgeeks.com/deploy-kubernetes-cluster-on-ubuntu-with-kubeadm/

https://www.golinuxcloud.com/calico-kubernetes/

<pre>
- name: CALICO_IPV4POOL_CIDR
  value: "10.142.0.0/24"

kubectl apply -f calico.yaml
</pre>

