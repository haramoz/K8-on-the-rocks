# Imperative Commands are Dope

--dry-run=client
By default, as soon as the command is run, the resource will be created. If you simply want to test your command, use the --dry-run=client option. This will not create the resource. Instead, tell you whether the resource can be created and if your command is right.

-o yaml This will output the resource definition in YAML format on the screen.

-l for labels

-ns for namespace

"The set of Pods targeted by a Service is usually determined by a selector that you define."

example:

apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app.kubernetes.io/name: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376


kubectl create configmap special-config --from-literal=special.how=very --from-literal=special.type=charm
kubectl create configmap cm-3392845 --from-literal=DB_NAME=SQL3322 --from-literal=DB_HOST=sql322.mycompany.com --from-literal=DB_PORT=3306


kubectl create secret generic db-user-pass \
    --from-literal=username=admin \
    --from-literal=password='S!B\*d$zDsb='

## Editing already deployed
kubectl replace -f <above yaml file> --force

kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.16.1

kubectl edit deployment/nginx-deployment

kubectl rollout status deployment/nginx-deployment

kubectl describe deployments
