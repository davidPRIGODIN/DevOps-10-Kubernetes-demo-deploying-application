# DevOps-10-Kubernetes-demo-deploying-application

This demo project demonstrates how to deploy a simple application setup in a Kubernetes cluster.

It consists of two applications: **MongoDB** and **Mongo Express**, illustrating a typical web application and its database.

## 1. Start Minikube

Start your local Kubernetes cluster by running:

```bash
minikube start
```

**Prerequisite:** If Minikube is not installed, follow the [official installation guide](https://minikube.sigs.k8s.io/docs/start/).

## 2. Deploy MongoDB

### 2.1. Configure Credentials

Encode the MongoDB username and password in Base64 to configure the Kubernetes Secret.

```bash
echo -n 'username' | base64
echo -n 'password' | base64
```

Add the encoded values to `mongo-secret.yaml`.

Note: Base64 is not encryption. Use third-party tools to encrypt Secrets before storing them in Git.

### 2.2. Create the Secret

Apply the Secret configuration and verify that it has been created:

```bash
kubectl apply -f mongo-secret.yaml
kubectl get secrets
```

### 2.3. Deploy MongoDB

Apply the MongoDB configuration and verify that the Service has been created:

```bash
kubectl apply -f mongo.yaml
kubectl get services
```

## 3. Deploy Mongo Express

### 3.1. Deploy the Application

Apply the ConfigMap and Mongo Express configurations:

```bash
kubectl apply -f mongo-configmap.yaml
kubectl apply -f mongo-express.yaml
```

Verify that the Service has been created:

```bash
kubectl get services
```

Access Mongo Express through Minikube:

```bash
minikube service mongo-express-service
```

### 3.2. Log In to Mongo Express

Retrieve the Mongo Express Pod name:

```bash
kubectl get pods
```

Check the Pod logs to find the authentication credentials:

```bash
kubectl logs <mongo-express-pod-name>
```

## 4. Verify the Deployment

### 4.1. Create a Database

In the Mongo Express web interface, create a new database named `Test-db`.

## 5. How It Works

When you click **Create Database**, a request is sent to the Mongo Express Service, which forwards it to the Mongo Express Pod.

The Mongo Express Pod then connects to the internal MongoDB Service, which forwards the request to the MongoDB Pod.

Once the database is created, the response travels back through the same path, and the changes are displayed in the Mongo Express web interface.

## Acknowledgements

This demo project was created as part of the DevOps Bootcamp by **TechWorld with Nana**.<br>
Many thanks to Nana for creating such a comprehensive and practical learning experience.
