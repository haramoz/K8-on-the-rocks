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
k -n mercury rollout history deploy cleaner --revision 1
k -n mercury rollout history deploy cleaner --revision 2


--restart
k -n moon rollout restart deploy web-moon


# Temp pod
k run tmp --restart=Never --rm --image=nginx:alpine -i -- curl http://project-plt-6cc-svc.pluto:3333
here .pluto at the end is because the temporary pod is not runnnig in pluto namespace.

k -n mars get pod -o wide # to get the cluster IP (Thats how you get the ip from the clusterIp connected to the pod)

Then: 
k run tmp --restart=Never --rm -i --image=nginx:alpine -- curl 10.0.0.67

For debugging svc its possible o investigate endpoints like this.
k -n mars get ep



# Run container in detach mode
podman run -d --name sun-cipher registry.killer.sh:5000/sun-cipher:v1-podman

# Get PV, PVC
pv, pvc can not be created impirically, but 
 k -n earth get pv,pvc can be done. And if a PV is connected to a PVC it shows connected status.

 k -n earth describe pod project-earthflower-d6887f7c5-pn5wv | grep -A2 Mounts:   
    Mounts:
      /tmp/project-data from data (rw) # there it is
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-n2sjj (ro)

# Configmanp
k -n moon create configmap configmap-web-moon-html --from-file=index.html=/opt/course/15/web-moon.html # important to set the index.html key

# Sidecar
# /opt/course/16/cleaner-new.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  name: cleaner
  namespace: mercury
spec:
  replicas: 2
  selector:
    matchLabels:
      id: cleaner
  template:
    metadata:
      labels:
        id: cleaner
    spec:
      volumes:
      - name: logs
        emptyDir: {}
      initContainers:
      - name: init
        image: bash:5.0.11
        command: ['bash', '-c', 'echo init > /var/log/cleaner/cleaner.log']
        volumeMounts:
        - name: logs
          mountPath: /var/log/cleaner
      - name: logger-con                                                # add
        image: busybox:1.31.0                                           # add
        restartPolicy: Always                                           # add
        command: ["sh", "-c", "tail -f /var/log/cleaner/cleaner.log"]   # add
        volumeMounts:                                                   # add
        - name: logs                                                    # add
          mountPath: /var/log/cleaner                                   # add
      containers:
      - name: cleaner-con
        image: bash:5.0.11
        args: ['bash', '-c', 'while true; do echo `date`: "remove random file" >> /var/log/cleaner/cleaner.log; sleep 1; done']
        volumeMounts:
        - name: logs
          mountPath: /var/log/cleaner

# logs
k -n mercury logs cleaner-576967576c-cqtgx -c logger-con

-c logger-con specifies the container within the pod from which you want to retrieve logs. In this case, the container is named logger-con.

## When to Use This Command:

You have a pod with multiple containers and need to fetch logs from a specific container.
You are working within a specific namespace and need to retrieve logs from a pod within that namespace.
You want to troubleshoot or monitor the logs of a containerized application running in Kubernetes.

## Additional Options:
Follow Logs: If you want to continuously stream the logs, you can add the -f option:

kubectl -n mercury logs cleaner-576967576c-cqtgx -c logger-con -f

View Logs from Previous Pod Instance: If you want to see logs from a previous instance of the container (e.g., after a restart), you can use:

kubectl -n mercury logs cleaner-576967576c-cqtgx -c logger-con --previous

