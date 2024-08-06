# Day 1:
I am going to write a certification on K8 in few weeks, so this is a preparation for that. I am going to have to practise daily.

## Commands
kubectl create deployment hello-node --image=registry.k8s.io/e2e-test-images/agnhost:2.39 -- /agnhost netexec --http-port=8080
kubectl get deployments
kubectl get pods
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
minikube service hello-node
minikube addons list
minikube addons enable metrics-server
kubectl get pod,svc -n kube-system

### cleanup
minikube addons disable metrics-server
kubectl delete service hello-node
kubectl delete deployment hello-node

minikube stop

### Test trials solution
k create deployment my-webapp --image=ngnix --replicas=2 -oyaml > my-webapp.yaml
k expose deployment my-webapp --name front-end-service --type NodePort --port 80 --dry-run=client -oyaml > front-end-service.yaml


## Aliases
alias k='kubectl'
alias kg='kubectl get'
alias kgpo='kubectl get pod'
alias krm='kubectl delete'
alias krmf='kubectl delete -f'

### Resources

https://kubernetes.io/docs/tutorials/hello-minikube/
https://kubernetes.io/docs/reference/kubectl/cheatsheet/
insane list of shortcuts
https://github.com/ahmetb/kubectl-aliases/blob/master/.kubectl_aliases 