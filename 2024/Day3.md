# Test Exams

kubectl get nodes --show-labels
kubectl label nodes <your-node-name> disktype=ssd


## ClaimRef
apiVersion: v1
kind: PersistentVolume
metadata:
  name: foo-pv
spec:
  storageClassName: ""
  claimRef:
    name: foo-pvc
    namespace: foo

## Helm basics 

## Probes

ReadinessProbe fails*: Pod won't be marked Ready and won't receive any traffic

LivenessProbe fails*: The container inside the Pod will be restarted

StartupProbe fails*: The container inside the Pod will be restarted

*fails: fails more times than configured with failureThreshold

## Scale
kubectl scale deploy wonderful-v1 --replicas 0

## Selectors in svc

## Docker

FROM bash
CMD ["ping", "killercoda.com"]

Build the image:

podman build -t pinger .

podman image ls

Run the image:

podman run --name my-ping pinger