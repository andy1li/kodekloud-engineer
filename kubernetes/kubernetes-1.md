# Kubernetes

## Level 1

### 1. Deploy Pods in Kubernetes Cluster

```bash
# thor@jump_host
vi pod.yml
kubectl apply -f pod.yml
kubectl get pods
```

```yaml
# pod.yml
apiVersion: v1
kind: Pod
metadata:
    name: pod-httpd
    labels:
      app: httpd_app
spec:
    containers:
    - name: httpd-container
      image: httpd:latest
```



### 2. Deploy Applications with Kubernetes Deployments

```bash
# thor@jump_host
kubectl create deployment httpd --image httpd:latest
kubectl get pods
kubectl get deployment
kubectl describe deployment/httpd
```



### 3. Setup Kubernetes Namespaces and PODs

```bash
# thor@jump_host
kubectl apply -f nginx.yml
kubectl get ns
kubectl get pods -n dev
```

```yaml
# nginx.yml
---
apiVersion: v1
kind: Namespace
metadata:
  name: dev
  
---
apiVersion: v1
kind: Pod
metadata:
  name: dev-nginx-pod
  namespace: dev
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
```



### 4. Set Resource Limits in Kubernetes Pods

```bash
# thor@jump_host
vi pod.yml
kubectl apply -f pod.yml
kubectl get pods
```

```yaml
# pod.yml
apiVersion: v1
kind: Pod
metadata:
    name: httpd-pod
spec:
    containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          memory: "15Mi"
          cpu: "100m"
        limits:
          memory: "20Mi"
          cpu: "100m"
```



### 5. Execute Rolling Updates in Kubernetes

```bash
# thor@jump_host
kubectl get all
kubectl get deployment nginx-deployment -o yaml
kubectl set image deployment/nginx-deployment nginx-container=nginx:1.19
watch kubectl get pods 
kubectl rollout status deployment nginx-deployment
kubectl describe deployment nginx-deployment
```



### 6. Revert Deployment to Previous Version in Kubernetes

```bash
# thor@jump_host
kubectl rollout undo deployment nginx-deployment
kubectl rollout status deployment nginx-deployment 
```



### 7. Deploy Replica Set in Kubernetes Cluster

```bash
# thor@jump_host
vi httpd.yml
kubectl apply -f httpd.yml
watch kubectl get pods
```

```yaml
# httpd.yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: httpd-replicaset
  labels:
    app: httpd_app
    type: front-end
spec:
  replicas: 4
  selector:
    matchLabels:
      app: httpd_app
  template:
    metadata:
      labels:
        app: httpd_app
        type: front-end
    spec:
      containers:
        - name: httpd-container
          image: httpd:latest
```



### 8. Schedule Cronjobs in Kubernetes

```bash
# thor@jump_host
vi cronjob.yml
kubectl get cronjob -A
```

```yaml
# cronjob.yml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: devops
spec:
  schedule: "*/6 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: cron-devops
              image: httpd:latest
              command:
                - /bin/sh
                - -c
                - echo Welcome to xfusioncorp
          restartPolicy: OnFailure  
```



### 9. Create Countdown Job in Kubernetes

```bash
# thor@jump_host
vi dummy.yml
kubectl apply -f dummy.yml 
kubectl get jobs
```

```yaml
# dummy.yml
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-devops
spec:
  template:
    metadata:
      name: countdown-devops
    spec:
      containers:
        - name: container-countdown-devops
          image: fedora:latest
          command: ["/bin/sh", "-c", "sleep 5"]
      restartPolicy: Never
```



### 10. Set Up Time Check Pod in Kubernetes

```bash
# thor@jump_host
kubectl get namespace
vi timechecker.yml
kubectl apply -f timechecker.yml
kubectl get pods -n nautilus
kubectl exec -n nautilus -it time-check -- tail /opt/itadmin/time/time-check.log
```

```yaml
# timechecker.yml
apiVersion: v1
kind: Namespace
metadata:
  name: nautilus
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: time-config
  namespace: nautilus
data:
  TIME_FREQ: "11"
---
apiVersion: v1
kind: Pod
metadata:
  name: time-check
  namespace: nautilus
  labels:
    app: time-check
spec:
  volumes:
    - name: log-volume
      emptyDir: {}
  containers:
    - name: time-check
      image: busybox:latest
      volumeMounts:
        - mountPath: /opt/itadmin/time
          name: log-volume
      envFrom:
        - configMapRef:
            name: time-config
      command: ["/bin/sh", "-c"]
      args:
        [
          "while true; do date; sleep $TIME_FREQ;done > /opt/itadmin/time/time-check.log",
        ] 
```

### 11. Resolve Pod Deployment Issue

```bash
# thor@jump_host
kubectl describe pods
kubectl get pods -o yaml > pod.yaml 
kubectl delete pod webserver
kubectl apply -f pod.yaml
watch kubectl get pods
```

```yaml
    containers:
    - image: nginx:latests
```



### 12. Update Deployment and Service in Kubernetes

```bash
# thor@jump_host
kubectl edit deployment nginx-deployment
watch kubectl get pods
kubectl describe deployment nginx-deployment

kubectl edit svc nginx-service
kubectl get svc
```
```yaml
# nginx-deployment
spec:
  replicas: 5
  template:
    spec:
      containers:
      - image: nginx:latest
# nginx-service
spec:
  ports:
  - nodePort: 32165
```



### 13. Deploy Highly Available Pods with Replication Controller

```bash
# thor@jump_host
vi replica-controller.yml
kubectl apply -f replica-controller.yml 
```

```yaml
# replica-controller.yml
apiVersion: v1
kind: ReplicationController
metadata:
  name: httpd-replicationcontroller
  labels:
    app: httpd_app
    type: front-end
spec:
  replicas: 3
  selector:
    app: httpd_app
  template:
    metadata:
      labels:
        app: httpd_app
        type: front-end
    spec:
      containers:
        - name: httpd-container
          image: 
            httpd:latest
          ports:
            - containerPort: 80 
```



### 14. Resolve Volume Mounts Issue in Kubernetes

```bash
# thor@jump_host
kubectl get pods
kubectl describe pods nginx-phpfpm

kubectl get pod/nginx-phpfpm -o yaml > pod.yml
kubectl get cm nginx-config -o yaml > pod-cm.yml

cat pod-cm.yml
vi pod.yml

kubectl replace -f pod.yml --force
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container 
```

```yaml
# pod-cm.yml
apiVersion: v1
data:
  nginx.conf: |
    http {
      server {
        # Set nginx to serve files from the shared volume!
        root /var/www/html;
        
# pod.yml
spec:
  containers:
  - image: nginx:latest
    volumeMounts:
    - mountPath: /var/www/html/
      name: shared-files
```



### Test

1. Test

```bash
# thor@jump_host

```
