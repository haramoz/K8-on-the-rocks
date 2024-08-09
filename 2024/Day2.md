# Scheduler
# Fast delete
kubectl delete pod app-sec-kff3345 --grace-period=0 --force

# Environment variable check
kubectl exec -it my-pod -- env
kubectl exec -it <pod-name> -c <container-name> -- env (multi container pod)
 
 # Security Context
 https://kubernetes.io/docs/tasks/configure-pod-container/security-context/

 apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo-4
spec:
  containers:
  - name: sec-ctx-4
    image: gcr.io/google-samples/hello-app:2.0
    securityContext:
      capabilities:
        add: ["NET_ADMIN", "SYS_TIME"]

# grep 
k -n pluto get pod,deployment | grep holy