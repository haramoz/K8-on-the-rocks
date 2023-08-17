# Day 5

## Deploying an python app

Python script: simple.py

#!/usr/bin/python3
import time
import socket

while True:
    host = socket.gethostname()
    date = time.strftime("%Y-%m-%d %H:%M:%S")
    now = str(date)

    f = open("date.out", "a")
    f.write(now + "\n")
    f.write(host + "\n")
    f.close()

    time.sleep(5)


Dockerfile:

FROM docker.io/library/python:3
ADD simple.py /
CMD [ "python", "./simple.py" ]

build:

docker build -t simpleapp .
docker run simpleapp

to find out the output file: (important)
sudo find / -name date.out

sudo tail /var/lib/docker/overlay2/57ceced953abca054b768804f21c8d90ef2c44eaf4776353e9004615269e2e0f/diff/date.out


----------------------------------------------------------------------------------------------------------------
Next day:
Scripts:

find $HOME -name easyregistry.yaml
kubectl create -f PATH_OF_EASYREGISTRY.YAML
kubectl get svc | grep registry
curl 10.97.40.62:5000/v2/_catalog

docker tag simpleapp:latest 10.97.40.62:5000/simpleapp:latest
docker push 10.97.40.62:5000/simpleapp:latest

After that:
curl 10.97.40.62:5000/v2/_catalog
{"repositories":["simpleapp"]}

kubectl create deployment try1 --image=$repo/simpleapp
kubectl scale deployment try1 --replicas=6
alias dpush="docker push 10.97.40.62:5000"
alias dpull="docker pull 10.97.40.62:5000"

kubectl delete deployment try1

check valid json -----> cat /etc/docker/daemon.json | jq 

-> easyregistry.yaml

apiVersion: v1
items:
- apiVersion: v1
  kind: Service
  metadata:
    annotations:
      kompose.cmd: kompose convert -f docker-compose.yaml -o localregistry.yaml
      kompose.version: 1.1.0 (36652f6)
    creationTimestamp: null
    labels:
      io.kompose.service: nginx
    name: nginx
  spec:
    ports:
    - name: "443"
      port: 443
      targetPort: 443
    selector:
      io.kompose.service: nginx
  status:
    loadBalancer: {}
- apiVersion: v1
  kind: Service
  metadata:
    annotations:
      kompose.cmd: kompose convert -f docker-compose.yaml -o localregistry.yaml
      kompose.version: 1.1.0 (36652f6)
    creationTimestamp: null
    labels:
      io.kompose.service: registry
    name: registry
  spec:
    clusterIP: 10.97.40.62
    ports:
    - name: "5000"
      port: 5000
      targetPort: 5000
    selector:
      io.kompose.service: registry
  status:
    loadBalancer: {}
- apiVersion: apps/v1
  kind: Deployment
  metadata:
    annotations:
      kompose.cmd: kompose convert -f docker-compose.yaml -o localregistry.yaml
      kompose.version: 1.1.0 (36652f6)
    creationTimestamp: null
    labels:
      io.kompose.service: nginx
    name: nginx
  spec:
    replicas: 1
    selector:
      matchLabels:
        io.kompose.service: nginx
    strategy:
      type: Recreate
    template:
      metadata:
        creationTimestamp: null
        labels:
          io.kompose.service: nginx
      spec:
        containers:
        - image: nginx:1.12
          name: nginx
          ports:
          - containerPort: 443
          resources: {}
          volumeMounts:
          - mountPath: /etc/nginx/conf.d
            name: nginx-claim0
        restartPolicy: Always
        volumes:
        - name: nginx-claim0
          persistentVolumeClaim:
            claimName: nginx-claim0
  status: {}
- apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    creationTimestamp: null
    labels:
      io.kompose.service: nginx-claim0
    name: nginx-claim0
  spec:
    accessModes:
    - ReadWriteOnce
    resources:
      requests:
        storage: 100Mi
  status: {}
- apiVersion: apps/v1
  kind: Deployment
  metadata:
    annotations:
      kompose.cmd: kompose convert -f docker-compose.yaml -o localregistry.yaml
      kompose.version: 1.1.0 (36652f6)
    creationTimestamp: null
    labels:
      io.kompose.service: registry
    name: registry
  spec:
    replicas: 1
    selector:
      matchLabels:
        io.kompose.service: registry
    strategy:
      type: Recreate
    template:
      metadata:
        creationTimestamp: null
        labels:
          io.kompose.service: registry
      spec:
        containers:
        - env:
          - name: REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY
            value: /data
          image: registry:2
          name: registry
          ports:
          - containerPort: 5000
            hostIP: 127.0.0.1
          resources: {}
          volumeMounts:
          - mountPath: /data
            name: registry-claim0
        restartPolicy: Always
        volumes:
        - name: registry-claim0
          persistentVolumeClaim:
            claimName: registry-claim0
  status: {}
- apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
    creationTimestamp: null
    labels:
      io.kompose.service: registry-claim0
    name: registry-claim0
  spec:
    accessModes:
    - ReadWriteOnce
    resources:
      requests:
        storage: 100Mi
  status: {}
- apiVersion: v1
  kind: PersistentVolume
  metadata:
    labels:
      type: local
    name: vol1 
  spec:
    accessModes:
    - ReadWriteOnce
    capacity:
      storage: 1Gi
    hostPath:
      path: /tmp/data
    persistentVolumeReclaimPolicy: Retain
- apiVersion: v1
  kind: PersistentVolume
  metadata:
    labels:
      type: local
    name: vol2
  spec:
    accessModes:
    - ReadWriteOnce
    capacity:
      storage: 200Mi
    hostPath:
      path: /tmp/nginx
    persistentVolumeReclaimPolicy: Retain
kind: List
metadata: {}

----------------------------------------------------------------------

Update: 

While playing with docker registry I broke the cluster!! Nothing is running and debugging right now. 
sudo systemctl restart kubelet
sudo systemctl status kubelet

<pre> kubelet.service - kubelet: The Kubernetes Node Agent
     Loaded: loaded (/lib/systemd/system/kubelet.service; enabled; vendor preset: enabled)
    Drop-In: /etc/systemd/system/kubelet.service.d
             └─10-kubeadm.conf
     Active: activating (auto-restart) (Result: exit-code) since Wed 2023-08-16 21:30:12 UTC; 6s ago
       Docs: https://kubernetes.io/docs/home/
    Process: 2497 ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS (code=exited, status=1/FAILURE)
   Main PID: 2497 (code=exited, status=1/FAILURE)
</pre>

sudo journalctl -u kubelet -n 500 --no-pager
sudo nano /lib/systemd/system/kubelet.service
sudo systemctl daemon-reload
sudo systemctl restart kubelet

sudo systemctl status docker
etcdctl cluster-health
systemctl status containerd   # If you're using containerd


cluster may be unhealthy: failed to list members
Error:  client: etcd cluster is unavailable or misconfigured; error #0: dial tcp 127.0.0.1:4001: connect: connection refused
; error #1: dial tcp 127.0.0.1:2379: connect: connection refused

error #0: dial tcp 127.0.0.1:4001: connect: connection refused
error #1: dial tcp 127.0.0.1:2379: connect: connection refused


Q: Is it easier to reinstall from scratch or try to fix the broken one ? 
A: I am extra motivated to fix the broken things so lets try this tonight, tomorrow might want to reinstall as the time is really short...

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/troubleshooting-kubeadm/

sudo kubeadm reset
sudo apt-get purge kubelet kubeadm kubectl
sudo rm -rf /etc/kubernetes/
sudo rm -rf /var/lib/kubelet/
sudo rm -rf /var/lib/kubeadm/

sudo rm -rf /etc/kubernetes/
sudo rm -rf /var/lib/kubelet/
sudo apt-get autoremove
sudo apt-get clean


sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

## reinstall
sudo apt-get install -y kubectl kubeadm kubelet kubernetes-cni 
-- docker is installed

sudo kubeadm config images pull

echo ‘{“exec-opts”: [“native.cgroupdriver=systemd”]}’ | sudo tee /etc/docker/daemon.json (This is wrong!! )
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl restart kubelet

----------------------------------------------
Failed to correct the system manually by deleting files, found an way to reset to the original state.

Go to the EC2 Dashboard -> Instances
Select the instance you want to reset.
Hit the dropdown menu for "Actions" -> "Monitor and troubleshoot" -> "Replace root volume"

HOLD
sudo apt-mark hold kubeadm kubelet kubectl

FIXES:
ls -l /var/lib/kubelet/config.yaml
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
https://stackoverflow.com/questions/62407918/kubelet-service-is-not-starting


kubelet was not running because the kubeadm init was not done correctly and the swap settings were not off. 

kubeadm join 172.31.43.209:6443 --token 6wbhkm.ecnzqumh5pnf9uim \
	--discovery-token-ca-cert-hash sha256:1b1a3c18d608ae68202317494e7be809793ed7d4905d8af43480d3c7616c20fb


-----------------------------------------------------------------------
Control plane is working i need to align the worker node to the new control plane

kubectl drain worker1 --ignore-daemonsets (Did not work)
kubectl delete node cp (Did not work)
sudo kubeadm reset

<pre>
W0817 12:37:00.240165    1975 preflight.go:56] [reset] WARNING: Changes made to this host by 'kubeadm init' or 'kubeadm join' will be reverted.
[reset] Are you sure you want to proceed? [y/N]: y
[preflight] Running pre-flight checks
W0817 12:37:04.452002    1975 removeetcdmember.go:106] [reset] No kubeadm config, using etcd pod spec to get data directory
[reset] Deleted contents of the etcd data directory: /var/lib/etcd
[reset] Stopping the kubelet service
[reset] Unmounting mounted directories in "/var/lib/kubelet"
[reset] Deleting contents of directories: [/etc/kubernetes/manifests /var/lib/kubelet /etc/kubernetes/pki]
[reset] Deleting files: [/etc/kubernetes/admin.conf /etc/kubernetes/kubelet.conf /etc/kubernetes/bootstrap-kubelet.conf /etc/kubernetes/controller-manager.conf /etc/kubernetes/scheduler.conf]

The reset process does not clean CNI configuration. To do so, you must remove /etc/cni/net.d

The reset process does not reset or clean up iptables rules or IPVS tables.
If you wish to reset iptables, you must do so manually by using the "iptables" command.

If your cluster was setup to utilize IPVS, run ipvsadm --clear (or similar)
to reset your system's IPVS tables.

The reset process does not clean your kubeconfig files and you must remove them manually.
Please, check the contents of the $HOME/.kube/config file.
</pre>
