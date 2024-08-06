# Day3 Agenda
I plan to install kubernetes cluster in my local machine. Using kubeadm and virtualbox. How hard that can be ? Lets figure that out. Turns out very hard. I 

Might be helpful:
https://forum.linuxfoundation.org/discussion/862825/kubeadm-init-error-cri-v1-runtime-api-is-not-implemented
## Steps

sudo kubeadm init --apiserver-advertise-address=172.31.44.164 --pod-network-cidr=10.244.0.0/16
kubectl apply -f https://github.com/coreos/flannel/raw/master/Documentation/kube-flannel.yml

IMPORTANT: Docker installation steps: https://forum.linuxfoundation.org/discussion/862825/kubeadm-init-error-cri-v1-runtime-api-is-not-implemented

in the worker node: sudo kubeadm join 172.31.44.164:6443 --token ll2j5a.a9he0y0g4uop1in3 --discovery-token-ca-cert-hash sha256:6b46ce99a0a7161e266bb953ff860c8152f89da3d8fcff9c89275ae1039fdf46 (changed)


### Links
Cluster installation guide for Ubuntu 20.04:  https://medium.com/@mehmetodabashi/installing-kubernetes-on-ubuntu-20-04-e49c43c63d0c

https://explore.skillbuilder.aws/learn?trk=8c070e03-5a8e-483f-b766-a157430938d6~ha_awssm-10292_tnc&sc_icampaign=aware_digitaltraining_skillbuilder_signin_mar_tnc_global_traincert_100-dl&sc_ichannel=ha&sc_icontent=awssm-10292_tnc&sc_iplace=signin
This is really nice resources link

How to skip pre-flight errors: sudo kubeadm join 172.31.44.164:6443 --token YOUR_TOKEN --discovery-token-ca-cert-hash YOUR_HASH --ignore-preflight-errors=FileAvailable--etc-kubernetes-kubelet.conf,Port-10250,FileAvailable--etc-kubernetes-pki-ca.crt


This is a AI link, not really relevant to K8
https://www.edx.org/learn/computer-programming/harvard-university-cs50s-introduction-to-artificial-intelligence-with-python
