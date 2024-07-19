# Dockerfile

# Build et Push de l'Image Docker

docker login

# Pousser l'Image vers Docker Hub
docker build -t laprofamacron/yourapp:latest

docker push laprofamacron/yourapp:latest

## Manifests Kubernetes

# deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: yourapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: yourapp
  template:
    metadata:
      labels:
        app: yourapp
    spec:
      containers:
      - name: yourapp
        image: yourusername/yourapp:latest
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "128Mi"
            cpu: "500m"
          limits:
            memory: "256Mi"
            cpu: "1"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /readiness
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10


# service.yaml

apiVersion: v1
kind: Service
metadata:
  name: yourapp-service
spec:
  selector:
    app: yourapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer

# Appliquez les manifests

kubectl apply -f redis-deployment.yaml
kubectl apply -f redis-service.yaml

# Vérifiez l'état des pods

kubectl get pods

# Vérifiez l'état des services

kubectl get services


## Explications

Workload et Service :

Deployment : Gère la réplication et la mise à l'échelle.
Service : Expose l'application à l'extérieur du cluster.
Configuration de l'Application :

Requests et Limits : Définit les ressources allouées et les limites.
Probes : Vérifie l'état de l'application pour assurer sa disponibilité.
Dépendances :

L'application utilise Python et les dépendances spécifiées dans requirements.txt. Aucune base de données ou cache externe n'est nécessaire pour le moment.
