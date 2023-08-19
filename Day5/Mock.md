# MOck test 
kubectl run messaging --image=redis:alpine --labels=tier=msg

generates the following 

apiVersion: v1
kind: Pod
metadata:
  name: messaging
  labels:
    tier: msg
spec:
  containers:
  - name: redis-container
    image: redis:alpine


kubectl get pods messaging --show-labels


kubectl create secret generic db-secret-xxdf \
  --from-literal=DB_Host=sql01 \
  --from-literal=DB_User=root \
  --from-literal=DB_Password=password123


kubectl get secret db-secret-xxdf -o yaml

Debugging:
kubectl describe pod rs-d33393-8r84l

Identify the Deployment Name:
If you don't already know the deployment name, you can list all deployments in the default namespace using:

bash
Copy code
<pre>kubectl get deployments</pre>
If you need to check deployments in a specific namespace, use:

bash
Copy code
<pre>kubectl get deployments -n <namespace-name></pre>
Edit the Deployment:

bash
Copy code
<pre>kubectl edit deployment <deployment-name></pre>
If the deployment is in a specific namespace, use:

bash
Copy code
<pre>kubectl edit deployment <deployment-name> -n <namespace-name></pre>

Monitor the Rollout:
After saving and exiting the editor, Kubernetes will start a rolling update based on the changes you made. You can monitor the progress of the rollout using:

bash
Copy code
<pre>kubectl rollout status deployment/<deployment-name></pre>
Revert If Necessary:
If something goes wrong or the new configuration isn't working as expected, you can roll back to a previous version of the deployment using:

bash
Copy code
<pre>kubectl rollout undo deployment/<deployment-name></pre>
By following these steps, you should be able to edit and update the configuration of a running deployment in a Kubernetes cluster.


Impirical commands:

kubectl -n marketing expose deployment redis --name messaging-service --port 6379 --target-port 6379

kubectl create service clusterip my-svc --clusterip="None" -o yaml --dry-run=client | kubectl set selector --local -f - 'environment=qa' -o yaml | kubectl create -f -


To fix a error in the replicaset they corrected the replicaset yaml first, then deleted the wrongly deployed pods. There is a easier way to delete the pods, using filtering by label.

<pre> k delete pod -l name=busybox-pod </pre>



