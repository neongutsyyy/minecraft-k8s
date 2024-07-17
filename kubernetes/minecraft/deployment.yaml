apiVersion: apps/v1
kind: Deployment
metadata:
  name: minecraft
spec:
  replicas: 1
  selector:
    matchLabels:
      app: minecraft
  template:
    metadata:
      labels:
        app: minecraft
    spec:
      containers:
      - name: minecraft
        image: neongutsyyy/minecraft-k8s:latest
        ports:
        - containerPort: 25565
        resources:
          requests:
            cpu: 500m
            memory: 1Gi
          limits:
            cpu: 2
            memory: 2Gi
