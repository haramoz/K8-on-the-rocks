# Elastic stack

## namespace
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: "2023-08-26T11:39:52Z"
  labels:
    kubernetes.io/metadata.name: elastic-stack
  name: elastic-stack
  resourceVersion: "774"
  uid: 650bb015-bc20-4b7d-86b7-a26beeb7e0a4
spec:
  finalizers:
  - kubernetes
status:
  phase: Active

 ## Kibana

 apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2023-08-26T11:40:21Z"
  labels:
    name: kibana
  name: kibana
  namespace: elastic-stack
  resourceVersion: "1015"
  uid: 44e9d5da-b4e5-426d-b41e-c4d196cde7bb
spec:
  containers:
  - env:
    - name: ELASTICSEARCH_URL
      value: http://elasticsearch:9200
    image: kibana:6.4.2
    imagePullPolicy: IfNotPresent
    name: kibana
    ports:
    - containerPort: 5601
      protocol: TCP
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-slmf4
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: controlplane
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: kube-api-access-slmf4
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T11:40:21Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T11:41:55Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T11:41:55Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T11:40:21Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: containerd://d36e086a59a8ee7bab164f69a43ec5494588c45c2d6cf94bab9a661e64cdffcf
    image: docker.io/library/kibana:6.4.2
    imageID: docker.io/library/kibana@sha256:989164ee9d20dfc5ba2d65339a7cf9309e75f6aaf92392fc875a47de8a733f97
    lastState: {}
    name: kibana
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2023-08-26T11:41:54Z"
  hostIP: 192.16.133.9
  phase: Running
  podIP: 10.244.0.7
  podIPs:
  - ip: 10.244.0.7
  qosClass: BestEffort
  startTime: "2023-08-26T11:40:21Z"

  ## App

  apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2023-08-26T12:32:18Z"
  labels:
    name: app
  name: app
  namespace: elastic-stack
  resourceVersion: "5037"
  uid: 33fcd22c-1df0-4a97-9e4f-57187faec0be
spec:
  containers:
  - image: kodekloud/event-simulator
    imagePullPolicy: Always
    name: app
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /log
      name: log-volume
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-wg6fh
      readOnly: true
  - image: kodekloud/filebeat-configured
    imagePullPolicy: Always
    name: sidecar
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/log/event-simulator/
      name: log-volume
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-wg6fh
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: controlplane
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - hostPath:
      path: /var/log/webapp
      type: DirectoryOrCreate
    name: log-volume
  - name: kube-api-access-wg6fh
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T12:32:18Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T12:32:24Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T12:32:24Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T12:32:18Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: containerd://c89b9251609e60e8f545fd465d81bfb6718953eca51c7a2ca8720990d7dd57c0
    image: docker.io/kodekloud/event-simulator:latest
    imageID: docker.io/kodekloud/event-simulator@sha256:1e3e9c72136bbc76c96dd98f29c04f298c3ae241c7d44e2bf70bcc209b030bf9
    lastState: {}
    name: app
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2023-08-26T12:32:20Z"
  - containerID: containerd://7044f1252b41614c118f05e707af61818876e9dc7912c6861ce9dd3fa9a01c44
    image: docker.io/kodekloud/filebeat-configured:latest
    imageID: docker.io/kodekloud/filebeat-configured@sha256:c49dab47599a42a3d80013b28d883bdd6276387e281de4ab96dddb6fbee360e7
    lastState: {}
    name: sidecar
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2023-08-26T12:32:23Z"
  hostIP: 192.16.133.9
  phase: Running
  podIP: 10.244.0.13
  podIPs:
  - ip: 10.244.0.13
  qosClass: BestEffort
  startTime: "2023-08-26T12:32:18Z"

  ## Elastic-search

  apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2023-08-26T11:40:20Z"
  labels:
    name: elastic-search
  name: elastic-search
  namespace: elastic-stack
  resourceVersion: "932"
  uid: b932f57f-ba21-4f85-bfd1-80a9a3ad36d3
spec:
  containers:
  - env:
    - name: discovery.type
      value: single-node
    image: docker.elastic.co/elasticsearch/elasticsearch:6.4.2
    imagePullPolicy: IfNotPresent
    name: elastic-search
    ports:
    - containerPort: 9200
      protocol: TCP
    - containerPort: 9300
      protocol: TCP
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-9gkrq
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  nodeName: controlplane
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: kube-api-access-9gkrq
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
status:
  conditions:
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T11:40:20Z"
    status: "True"
    type: Initialized
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T11:41:04Z"
    status: "True"
    type: Ready
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T11:41:04Z"
    status: "True"
    type: ContainersReady
  - lastProbeTime: null
    lastTransitionTime: "2023-08-26T11:40:20Z"
    status: "True"
    type: PodScheduled
  containerStatuses:
  - containerID: containerd://45ad7a746ead0524c566df86c78d251e845a609f209de8ebc8ed0fcec11d598d
    image: docker.elastic.co/elasticsearch/elasticsearch:6.4.2
    imageID: docker.elastic.co/elasticsearch/elasticsearch@sha256:3da16b2f3b1d4e151c44f1a54f4f29d8be64884a64504b24ebcbdb4e14c80aa1
    lastState: {}
    name: elastic-search
    ready: true
    restartCount: 0
    started: true
    state:
      running:
        startedAt: "2023-08-26T11:41:04Z"
  hostIP: 192.16.133.9
  phase: Running
  podIP: 10.244.0.6
  podIPs:
  - ip: 10.244.0.6
  qosClass: BestEffort
  startTime: "2023-08-26T11:40:20Z" 