# Preparion :
This proven to be the weakest point in my armour
- Application Environment, Configuration and Security (25%)
- Application Design and Build (20%)
- Application Deployment (20%)

## Game Plan
Solve the killer.sh shell today. Tomorrow, do another Time bound trial.

## Areas I need to improve
Service accounts
canary deployment
pinging from a temporary pod for networking testing
add security as environment variable
add networkpolicy
sidecar
resources
max surge
cronjob


### Debug Secrets
➜ k -n moon exec secret-handler -- env | grep SECRET1
SECRET1_USER=test
SECRET1_PASS=pwd

➜ k -n moon exec secret-handler -- find /tmp/secret2 
/tmp/secret2
/tmp/secret2/..data
/tmp/secret2/key
/tmp/secret2/..2019_09_11_09_03_08.147048594
/tmp/secret2/..2019_09_11_09_03_08.147048594/key

➜ k -n moon exec secret-handler -- cat /tmp/secret2/key
12345678

### ingress
kubectl create ingress ingress --rule="ckad-mock-exam-solution.com/video*=my-video-service:8080" --dry-run=client -oyaml > ingress.yaml

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
  name: ingress
spec:
  rules:
  - host: ckad-mock-exam-solution.com
    http:
      paths:
      - backend:
          service:
            name: my-video-service
            port:
              number: 8080
        path: /video
        pathType: Prefix

### status
kubectl -n default get pod pod1 -o jsonpath="{.status.phase}"
k -n venus exec frontend-789cbdc677-c9v8h -- wget -O- api:2222