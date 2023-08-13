# Agenda Day 4

### Scripts

sudo apt-mark hold kubelet kubeadm kubectl (holds the current version)
sudo apt-get install bash-completion vim -y (installs useful apps)

kubectl taint --help
kubectl describe nodes | grep -i taint
kubectl taint nodes --all node-role.kubernetes.io/control-plane-

service:

apiVersion: v1
kind: Service
metadata:
  name: basicservice
spec:
  selector:
    type: webserver
  type: NodePort
  ports:
  - protocol: TCP
    port: 80

curl ifconfig.io --> public ip of the nodeport

---------------------

kubectl create deployment firstpod --image=nginx
kubectl get deployment,pod
kubectl describe deployment firstpod
kubectl get namespaces
kubectl get pod -n kube-system
kubectl get pod --all-namespaces
kubectl get deploy,rs,po,svc,ep
-----------------------------------------------------------
curl -I https://registry-1.docker.io/v2/ check connectivity to the docker registry


### Refs

https://medium.com/bb-tutorials-and-thoughts/practice-enough-with-these-questions-for-the-ckad-exam-2f42d1228552


