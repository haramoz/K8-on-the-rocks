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

