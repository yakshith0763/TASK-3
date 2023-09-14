Dockerize Your Java Application:

Create a Dockerfile for your Java application. Here's an example Dockerfile for a Spring Boot application:
# Use the official OpenJDK base image
FROM openjdk:11-jre-slim

# Set the working directory
WORKDIR /app

# Copy the JAR file into the container
COPY target/your-app.jar /app/your-app.jar

# Expose the application's port
EXPOSE 8080

# Run the application
CMD ["java", "-jar", "your-app.jar"]
Replace your-app.jar with the name of your application's JAR file.

Build the Docker image for your application:
docker build -t your-app-image .
mongodb-deployment.yaml (MongoDB):
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
        - name: mongodb
          image: mongo:latest
          ports:
            - containerPort: 27017
          volumeMounts:
            - name: mongodb-storage
              mountPath: /data/db
      volumes:
        - name: mongodb-storage
          persistentVolumeClaim:
            claimName: mongodb-pvc
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongodb-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi  # Adjust the storage size as needed
your-app-deployment.yaml (Your Java Application):
apiVersion: apps/v1
kind: Deployment
metadata:
  name: your-app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: your-app
  template:
    metadata:
      labels:
        app: your-app
    spec:
      containers:
        - name: your-app
          image: your-app-image:latest
          ports:
            - containerPort: 8080
          env:
            - name: MONGO_URI
              value: "mongodb://mongodb-service:27017/yourdb"  # Update with your MongoDB connection URI
---
apiVersion: v1
kind: Service
metadata:
  name: your-app-service
spec:
  selector:
    app: your-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer  # Use LoadBalancer, NodePort, or Ingress based on your environment
Deploy MongoDB:

You can deploy MongoDB using Helm or any other preferred method. For example, to deploy MongoDB using Helm:
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install mongodb bitnami/mongodb
Apply Kubernetes Manifests:

Apply your Kubernetes manifests:
kubectl apply -f mongodb-deployment.yaml
kubectl apply -f your-app-deployment.yaml
