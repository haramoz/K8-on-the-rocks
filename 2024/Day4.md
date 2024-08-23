# Jobs

# Service accounts

# Memory 
apiVersion: v1
kind: Pod
metadata:
  name: constraints-mem-demo
spec:
  containers:
  - name: constraints-mem-demo-ctr
    image: nginx
    resources:
      limits:
        memory: "800Mi"
      requests:
        memory: "600Mi"

# Get Status
The content of the command file could look like:

kubectl -n default describe pod pod1 | grep -i status:

Another solution would be using jsonpath:

kubectl -n default get pod pod1 -o jsonpath="{.status.phase}"

kubectl get pod test -o json --> this returns the json path

kubectl explain pod.status.phase


# Helm

helm -n mercury ls -a 
( By default releases in pending-upgrade state aren't listed, but we can show all to find and delete the broken release )

helm -n mercury install internal-issue-report-apache bitnami/apache \
  --set replicaCount=2 \
  --set image.debug=true

helm -n mercury upgrade internal-issue-report-apiv2 bitnami/nginx

helm repo list
helm repo update
helm search repo nginx

helm -n mercury ls

# base64
echo "SGVsbG8gV29ybGQh" | base64 -d --> for decoding
echo -n "Hello World!" | base64 --> for encoding

# Rollout

k -n neptune rollout history deploy api-new-c32


# Temp pod
k run tmp --restart=Never --rm --image=nginx:alpine -i -- curl http://project-plt-6cc-svc.pluto:3333

# Run container in detach mode
podman run -d --name sun-cipher registry.killer.sh:5000/sun-cipher:v1-podman

