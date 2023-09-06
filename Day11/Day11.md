# Cronjob

## requirements

- I want this to run on the weekdays at 8 am.
- I want this to start and stop the mongodb database
- I need to design PV and PVC for the mongodb database

## Implementations

### Cronjob
<pre>
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: mongodb-job
spec:
  schedule: "0 8 * * 1-5" # Every weekday at 8am
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: mongodb
            image: mongo:4.4.19
            env:
            - name: MONGO_INITDB_ROOT_DATABASE
              value: lead_generation_db
            - name: MONGO_INITDB_ROOT_USERNAME
              valueFrom:
                secretKeyRef:
                  name: mongodb-credentials
                  key: MONGO_INITDB_ROOT_USERNAME
            - name: MONGO_INITDB_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mongodb-credentials
                  key: MONGO_INITDB_ROOT_PASSWORD
            ports:
            - containerPort: 27017
            volumeMounts:
            - name: mongo-data
              mountPath: /data/db
            - name: init-mongo-script
              mountPath: /docker-entrypoint-initdb.d
          volumes:
          - name: mongo-data
            persistentVolumeClaim:
              claimName: mongodb-pvc
          - name: init-mongo-script
            configMap:
              name: init-mongo-script
          restartPolicy: OnFailure
</pre>

### Persistent Volume Claim

<pre>
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mongodb-pv
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/path/on/host"
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongodb-pvc
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi

</pre>

### Configmap for the initiation

<pre>
kubectl create configmap init-mongo-script --from-file=../backend/mongoDB/init-mongo.js

</pre>

### Secrets

<pre>
kubectl create secret generic mongodb-credentials \
  --from-literal=MONGO_INITDB_ROOT_USERNAME=admin \
  --from-literal=MONGO_INITDB_ROOT_PASSWORD=admin
</pre>




