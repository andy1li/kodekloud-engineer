# Kubernetes

## Level 2

### 1. Kubernetes Shared Volumes

```bash
# thor@jump_host
vi deploy.yml
kubectl apply -f .

kubectl exec -it volume-share-devops -c volume-container-devops-1 -- touch /tmp/blog/blog.txt
kubectl exec -it volume-share-devops -c volume-container-devops-1  -- ls -lah /tmp/blog
kubectl exec -it volume-share-devops -c volume-container-devops-2  -- ls -lah /tmp/apps
```

```yaml
# deploy.yml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
  labels:
    name: devops
spec:
  volumes:
    - name: volume-share
      emptyDir: {}
  containers:
    - name: volume-container-devops-1
      image: debian:latest
      command: ["/bin/bash", "-c", "sleep 10000"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/blog
    - name: volume-container-devops-2
      image: debian:latest
      command: ["/bin/bash", "-c", "sleep 10000"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/apps
```



### 2. Kubernetes Sidecar Containers

```bash
# thor@jump_host
vi sidecar.yml
kubectl apply -f sidecar.yml
```

```yaml
# sidecar.yml
apiVersion: v1
kind: Pod
metadata:
  name: webserver
  labels:
    name: webserver
spec:
  volumes:
    - name: shared-logs
      emptyDir: {}
  containers:
    - name: nginx-container
      image: nginx:latest
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx
    - name: sidecar-container
      image: ubuntu:latest
      command:
        [
          "sh",
          "-c",
          "while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"
        ]
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx
```



### 3. Deploy Nginx Web Server on Kubernetes Cluster

```bash
# thor@jump_host
vi nginx.yml
kubectl apply -f nginx.yml
watch kubectl get all
```

```yaml
# nginx.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx-app
    type: front-end
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-app
      type: front-end
  template:
    metadata:
      labels:
        app: nginx-app
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest  
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx-app
    type: front-end
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30011
```



### 4. Print Environment Variables

```bash
# thor@jump_host
vi print.yml
kubectl apply -f print.yml
kubectl logs print-envars-greeting
```

```yaml
# print.yml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
  labels:
    name: print-envars-greeting
spec:
  restartPolicy: Never
  containers:
    - name: print-env-container
      image: bash
      env:
        - name: GREETING
          value: "Welcome to"
        - name: COMPANY
          value: "Nautilus"
        - name: GROUP
          value: "Ltd"
      command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
```



### 5. Rolling Updates And Rolling Back Deployments in Kubernetes

```bash
# thor@jump_host
kubectl create namespace devops
kubectl get ns

vi app.yml
kubectl apply -f app.yml
kubectl get all -n devops

kubectl set image deployment httpd-deploy  httpd=httpd:2.4.43 --namespace devops --record=true
kubectl describe -n devops deployment/httpd-deploy

kubectl rollout undo deployment httpd-deploy -n devops 
kubectl describe -n devops deployment/httpd-deploy
```

```yaml
# app.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd-deploy
  namespace: devops
spec:
  replicas: 4
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 2
  selector:
    matchLabels:
      app: httpd
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
        - name: httpd
          image: httpd:2.4.27
---
apiVersion: v1
kind: Service
metadata:
  name: httpd-service
  namespace: devops
spec:
  type: NodePort
  selector:
    app: httpd
  ports:
    - nodePort: 30008          
      port: 80
      targetPort: 80
```



### 6. Deploy Jenkins on Kubernetes

```bash
# thor@jump_host
kubectl create namespace jenkins
kubectl get ns

vi jenkins.yml
kubectl apply -f jenkins.yml

kubectl get all -n jenkins
```

```yaml
# jenkins.yml
apiVersion: v1
kind: Namespace
metadata:
  name: jenkins
  labels:
    app: jenkins
---
apiVersion: v1
kind: Service
metadata:
  name: jenkins-service
  namespace: jenkins
spec:
  type: NodePort
  selector:
    app: jenkins
  ports:
    - nodePort: 30008          
      port: 8080
      targetPort: 8080
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins-deployment
  namespace: jenkins
  labels:
     app: jenkins
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      containers:
        - name: jenkins-container
          image: jenkins/jenkins
          ports:
            - containerPort: 8080
```



### 7. Deploy Grafana on Kubernetes Cluster

```bash
# thor@jump_host
vi grafana.yml
kubectl apply -f grafana.yml
```

```yaml
# grafana.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-xfusion
  labels:
     app: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana-container-nautilus
        image: grafana/grafana:latest
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 3000
          name: http-grafana
        resources:
          requests:
            cpu: 500m
            memory: 1Gi
          # limits:
          #   cpu: “1000m”
          #   memory: “2Gi”
        volumeMounts:
        - mountPath: /var/lib/grafana
          name: grafana-pv
      volumes:
      - name: grafana-pv
        emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: grafana-deployment-nautilus
  labels:
     app: grafana
spec:
  type: NodePort
  ports:
    - port: 3000
      targetPort: 3000
      nodePort: 32000
      protocol: TCP
  selector:
    app: grafana
```



### 8. Deploy Tomcat App on Kubernetes

```bash
# thor@jump_host
vi tomcat.yml
kubectl apply -f tomcat.yml

kubectl get all -n tomcat-namespace-devops
```

```yaml
# tomcat.yml
apiVersion: v1
kind: Namespace
metadata:
  name: tomcat-namespace-devops
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: tomcat-deployment-devops
  namespace: tomcat-namespace-devops
spec:
  replicas: 1
  selector:
    matchLabels:
      app: tomcat
  template:
    metadata:
      labels:
        app: tomcat
    spec:
      containers:
        - name: tomcat-container-devops
          image: gcr.io/kodekloud/centos-ssh-enabled:tomcat
          ports:
            - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: tomcat-service-devops
  namespace: tomcat-namespace-devops
spec:
  type: NodePort
  selector:
    app: tomcat
  ports:
    - port: 80
      protocol: TCP
      targetPort: 8080
      nodePort: 32227
```



### 9. Deploy Node App on Kubernetes

```bash
# thor@jump_host
vi node.yml
kubectl apply -f node.yml

watch kubectl get all
```

```yaml
# node.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
        - name: node-container
          image: gcr.io/kodekloud/centos-ssh-enabled:node
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: node-app
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30012
```



### 10. Troubleshoot Deployment issues in Kubernetes

```bash
# thor@jump_host
kubectl get all
kubectl describe pod/redis-deployment-6fd9d5fcb-8qz2b
kubectl get cm

kubectl edit deployment redis-deployment

kubectl logs -f deployment/redis-deployment
```

```yaml
# kubectl edit deployment redis-deployment
name: config
- image: redis:alpine
```



### 11. Fix issue with LAMP Environment in Kubernetes

```bash
# thor@jump_host
k get all
k describe service/lamp-service

k edit service/lamp-service
# change 8080 -> 80: %s/8080/80/g

HTTP=$(kubectl get pod  -o=jsonpath='{.items[*].spec.containers[0].name}'); echo "HTTP: $HTTP"
MYSQL=$(kubectl get pod -o=jsonpath='{.items[*].spec.containers[1].name}'); echo "MYSQL: $MYSQL"
POD=$(kubectl get pods -o=jsonpath='{.items[*].metadata.name}'); echo "POD: $POD"

k logs -f $POD -c $HTTP 

k exec -it $POD -c $HTTP -- bash
ls
ls app
# index.php
vi app/index.php
# $dbpass = $_ENV['MYSQL_PASSWORD'];
# $dbhost = $_ENV['MYSQL_HOST'];
```
