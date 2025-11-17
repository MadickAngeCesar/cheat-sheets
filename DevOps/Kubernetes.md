# Kubernetes Cheat Sheet - Beginner to Pro

## Table of Contents
- [Setup & Installation](#setup--installation)
- [Basic Commands](#basic-commands)
- [Pods](#pods)
- [Deployments](#deployments)
- [Services](#services)
- [ConfigMaps](#configmaps)
- [Secrets](#secrets)
- [Namespaces](#namespaces)
- [Volumes](#volumes)
- [StatefulSets](#statefulsets)
- [DaemonSets](#daemonsets)
- [Jobs & CronJobs](#jobs--cronjobs)
- [Ingress](#ingress)
- [Helm](#helm)
- [Best Practices](#best-practices)

---

## Setup & Installation

### Install kubectl
```bash
# Linux
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# macOS
brew install kubectl

# Windows
choco install kubernetes-cli

# Verify installation
kubectl version --client
```

### Install Minikube (Local Development)
```bash
# Linux
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# macOS
brew install minikube

# Windows
choco install minikube

# Start minikube
minikube start

# Check status
minikube status

# Stop minikube
minikube stop
```

---

## Basic Commands

### Cluster Info
```bash
# View cluster info
kubectl cluster-info

# View nodes
kubectl get nodes

# Describe node
kubectl describe node <node-name>

# View API resources
kubectl api-resources

# View API versions
kubectl api-versions
```

### Context & Configuration
```bash
# View contexts
kubectl config get-contexts

# Current context
kubectl config current-context

# Switch context
kubectl config use-context <context-name>

# View config
kubectl config view

# Set namespace
kubectl config set-context --current --namespace=<namespace>
```

---

## Pods

### Basic Pod Operations
```bash
# List pods
kubectl get pods

# List all pods in all namespaces
kubectl get pods --all-namespaces
kubectl get pods -A

# Describe pod
kubectl describe pod <pod-name>

# View pod logs
kubectl logs <pod-name>

# Follow logs
kubectl logs -f <pod-name>

# Previous logs
kubectl logs --previous <pod-name>

# Logs from specific container
kubectl logs <pod-name> -c <container-name>

# Execute command in pod
kubectl exec <pod-name> -- ls /app

# Interactive shell
kubectl exec -it <pod-name> -- /bin/bash

# Delete pod
kubectl delete pod <pod-name>

# Delete all pods
kubectl delete pods --all
```

### Pod YAML
```yaml
# pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.21
    ports:
    - containerPort: 80
    env:
    - name: ENV_VAR
      value: "value"
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

### Apply Pod
```bash
# Create/update pod
kubectl apply -f pod.yaml

# Create from command
kubectl run nginx --image=nginx:1.21

# Create with port
kubectl run nginx --image=nginx:1.21 --port=80

# Create with labels
kubectl run nginx --image=nginx:1.21 --labels="app=web,env=prod"
```

---

## Deployments

### Deployment Commands
```bash
# List deployments
kubectl get deployments
kubectl get deploy

# Describe deployment
kubectl describe deployment <deployment-name>

# Create deployment
kubectl create deployment nginx --image=nginx:1.21

# Scale deployment
kubectl scale deployment nginx --replicas=3

# Autoscale
kubectl autoscale deployment nginx --min=2 --max=10 --cpu-percent=80

# Update image
kubectl set image deployment/nginx nginx=nginx:1.22

# Rollout status
kubectl rollout status deployment/nginx

# Rollout history
kubectl rollout history deployment/nginx

# Rollback
kubectl rollout undo deployment/nginx

# Rollback to specific revision
kubectl rollout undo deployment/nginx --to-revision=2

# Delete deployment
kubectl delete deployment nginx
```

### Deployment YAML
```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
```

---

## Services

### Service Commands
```bash
# List services
kubectl get services
kubectl get svc

# Describe service
kubectl describe service <service-name>

# Expose deployment
kubectl expose deployment nginx --port=80 --type=LoadBalancer

# Delete service
kubectl delete service nginx
```

### Service Types

#### ClusterIP (Internal)
```yaml
# service-clusterip.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
```

#### NodePort (External via Node)
```yaml
# service-nodeport.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30080  # 30000-32767
```

#### LoadBalancer (Cloud)
```yaml
# service-loadbalancer.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
```

---

## ConfigMaps

### ConfigMap Commands
```bash
# List configmaps
kubectl get configmaps
kubectl get cm

# Describe configmap
kubectl describe configmap <configmap-name>

# Create from literal
kubectl create configmap app-config --from-literal=KEY=value

# Create from file
kubectl create configmap app-config --from-file=config.properties

# Delete configmap
kubectl delete configmap app-config
```

### ConfigMap YAML
```yaml
# configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  DATABASE_HOST: "postgres"
  DATABASE_PORT: "5432"
  config.json: |
    {
      "key": "value"
    }
```

### Using ConfigMap in Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app
    image: myapp:1.0
    # Environment variables from ConfigMap
    envFrom:
    - configMapRef:
        name: app-config
    # Specific environment variable
    env:
    - name: DB_HOST
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: DATABASE_HOST
    # Mount as volume
    volumeMounts:
    - name: config-volume
      mountPath: /etc/config
  volumes:
  - name: config-volume
    configMap:
      name: app-config
```

---

## Secrets

### Secret Commands
```bash
# List secrets
kubectl get secrets

# Describe secret
kubectl describe secret <secret-name>

# Create from literal
kubectl create secret generic db-secret --from-literal=password=mypassword

# Create from file
kubectl create secret generic db-secret --from-file=./password.txt

# Create TLS secret
kubectl create secret tls tls-secret --cert=cert.crt --key=cert.key

# Delete secret
kubectl delete secret db-secret
```

### Secret YAML
```yaml
# secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=      # base64 encoded
  password: cGFzc3dvcmQ=  # base64 encoded

# Encode/decode
# echo -n 'admin' | base64
# echo 'YWRtaW4=' | base64 -d
```

### Using Secret in Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
  - name: app
    image: myapp:1.0
    # Environment variables from Secret
    envFrom:
    - secretRef:
        name: db-secret
    # Specific environment variable
    env:
    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password
    # Mount as volume
    volumeMounts:
    - name: secret-volume
      mountPath: /etc/secret
      readOnly: true
  volumes:
  - name: secret-volume
    secret:
      secretName: db-secret
```

---

## Namespaces

### Namespace Commands
```bash
# List namespaces
kubectl get namespaces
kubectl get ns

# Create namespace
kubectl create namespace dev

# Delete namespace
kubectl delete namespace dev

# Set default namespace
kubectl config set-context --current --namespace=dev

# Get resources in namespace
kubectl get pods -n dev

# Get resources in all namespaces
kubectl get pods --all-namespaces
kubectl get pods -A
```

### Namespace YAML
```yaml
# namespace.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

---

## Volumes

### Volume Types

#### EmptyDir (Temporary)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-volume
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: cache
      mountPath: /cache
  volumes:
  - name: cache
    emptyDir: {}
```

#### HostPath (Node Directory)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-hostpath
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: data
      mountPath: /data
  volumes:
  - name: data
    hostPath:
      path: /mnt/data
      type: Directory
```

#### PersistentVolume & PersistentVolumeClaim
```yaml
# persistent-volume.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-data
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: standard
  hostPath:
    path: /mnt/data

---
# persistent-volume-claim.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-data
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: standard

---
# pod-with-pvc.yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-pvc
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - name: data
      mountPath: /data
  volumes:
  - name: data
    persistentVolumeClaim:
      claimName: pvc-data
```

---

## StatefulSets

### StatefulSet YAML
```yaml
# statefulset.yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi
```

---

## DaemonSets

### DaemonSet YAML
```yaml
# daemonset.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
spec:
  selector:
    matchLabels:
      app: fluentd
  template:
    metadata:
      labels:
        app: fluentd
    spec:
      containers:
      - name: fluentd
        image: fluentd:latest
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
```

---

## Jobs & CronJobs

### Job YAML
```yaml
# job.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: backup-job
spec:
  template:
    spec:
      containers:
      - name: backup
        image: backup-tool:latest
        command: ["/bin/sh", "-c", "backup.sh"]
      restartPolicy: OnFailure
  backoffLimit: 4
```

### CronJob YAML
```yaml
# cronjob.yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: backup-cronjob
spec:
  schedule: "0 2 * * *"  # Every day at 2 AM
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: backup
            image: backup-tool:latest
            command: ["/bin/sh", "-c", "backup.sh"]
          restartPolicy: OnFailure
```

---

## Ingress

### Ingress YAML
```yaml
# ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
  - host: api.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 8080

---
# With TLS
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress-tls
spec:
  tls:
  - hosts:
    - example.com
    secretName: tls-secret
  rules:
  - host: example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80
```

---

## Helm

### Helm Installation
```bash
# Install Helm
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Verify
helm version
```

### Helm Commands
```bash
# Add repository
helm repo add stable https://charts.helm.sh/stable

# Update repositories
helm repo update

# Search charts
helm search repo nginx

# Install chart
helm install my-release stable/nginx

# Install with custom values
helm install my-release stable/nginx -f values.yaml

# List releases
helm list

# Get release info
helm status my-release

# Upgrade release
helm upgrade my-release stable/nginx

# Rollback release
helm rollback my-release 1

# Uninstall release
helm uninstall my-release

# Create new chart
helm create mychart

# Package chart
helm package mychart/

# Install local chart
helm install my-release ./mychart
```

### Chart Structure
```
mychart/
├── Chart.yaml
├── values.yaml
├── charts/
└── templates/
    ├── deployment.yaml
    ├── service.yaml
    └── ingress.yaml
```

### values.yaml
```yaml
replicaCount: 3

image:
  repository: nginx
  tag: "1.21"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80

resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi
```

---

## Best Practices

### Resource Management
```yaml
# ✅ Good: Set resource requests and limits
spec:
  containers:
  - name: app
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"

# ✅ Good: Use readiness and liveness probes
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
```

### Security
```yaml
# ✅ Good: Run as non-root
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
  containers:
  - name: app
    securityContext:
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true

# ✅ Good: Use secrets for sensitive data
env:
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password

# ✅ Good: Network policies
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```

### High Availability
```yaml
# ✅ Good: Multiple replicas
spec:
  replicas: 3

# ✅ Good: Pod disruption budget
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: app-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: myapp

# ✅ Good: Anti-affinity
spec:
  affinity:
    podAntiAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
          - key: app
            operator: In
            values:
            - myapp
        topologyKey: kubernetes.io/hostname
```

---

**Pro Tip:** Use namespaces to organize resources, always set resource requests and limits, implement health checks with liveness and readiness probes, use Helm for complex application deployments, and leverage ConfigMaps and Secrets for configuration management!