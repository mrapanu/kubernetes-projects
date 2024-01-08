# Kubernetes Projects

This repository contains the Kubernetes configuration files for various projects. Below is the documentation for each project:

1. [Dashy](#dashy)
2. [Portainer](#portainer)

## Dashy

[Dashy](https://github.com/Lissy93/dashy) is an open source, highly customizable, easy to use, privacy-respecting dashboard app. Below are the details and deployment steps in kubernetes:

### Project Structure

- `dashy-depl.yaml`: Deployment configuration for Dashy application.
- `dashy-namespace.yaml`: Namespace configuration for Dashy.
- `dashy-pv.yaml`: PersistentVolume configuration for storing Dashy configuration.
- `dashy-pvc.yaml`: PersistentVolumeClaim configuration for accessing Dashy configuration storage.
- `dashy-svc.yaml`: Service configuration for exposing Dashy application.

### Prerequisites

Before deploying Dashy, make sure you have the following prerequisites:

1. Kubernetes cluster is up and running.
2. `kubectl` command-line tool is installed.
3. NFS server is available with the required shared path. `IMPORTANT`: Change dashy-pv.yaml server and path section with your needs.

### Deployment Steps

   ```
   kubectl apply -f dashy-namespace.yaml  #Create Namespace
   kubectl apply -f dashy-pv.yaml #Create Persistent Volume 
   kubectl apply -f dashy-pvc.yaml #Create Persistent Volume Claim
   kubectl apply -f dashy-depl.yaml #Deploy Dashy Application
   kubectl apply -f dashy-svc.yaml #Expose Dashy application using a LB
   ```

You can deploy all at once as follows:

   ```
   cd Dashy/
   kubectl apply -f .
   ```
Wait for the LoadBalancer service to obtain an external IP address. Check the status with:

```
kubectl get svc -n dashy
```
Once the external IP is assigned, you can access Dashy at  `http://EXTERNAL_IP:8000`.

### Clean Up

To remove the Dashy deployment and associated resources, run the following commands:

```
kubectl delete -f dashy-svc.yaml
kubectl delete -f dashy-depl.yaml
kubectl delete -f dashy-pvc.yaml
kubectl delete -f dashy-pv.yaml
kubectl delete -f dashy-namespace.yaml
```
You can delete all at once as follows:
   ```
   cd Dashy/
   kubectl delete -f .
   ```
## Portainer

Portainer is a container management tool deployed on a Kubernetes cluster. Below are the details and deployment steps:

### Project Structure

- `portainer-depl.yaml`: Deployment configuration for Portainer application.
- `portainer-namespace.yaml`: Namespace configuration for Portainer.
- `portainer-pv.yaml`: PersistentVolume configuration for storing Portainer configuration.
- `portainer-pvc.yaml`: PersistentVolumeClaim configuration for accessing Portainer configuration storage.
- `portainer-rbac.yaml`: ClusterRoleBinding configuration for Portainer.
- `portainer-sa.yaml`: ServiceAccount configuration for Portainer.
- `portainer-svc.yaml`: Service configuration for exposing Portainer application.

### Prerequisites

Before deploying Portainer, make sure you have the following prerequisites:

1. Kubernetes cluster is up and running.
2. `kubectl` command-line tool is installed.
3. NFS server is available with the required shared path. `IMPORTANT`: Change dashy-pv.yaml server and path section with your needs.

### Deployment Steps

```
kubectl apply -f portainer-namespace.yaml #Create namespace
kubectl apply -f portainer-pv.yaml #Create Persistent Volume
kubectl apply -f portainer-pvc.yaml #Create Persistent Volume Claim
kubectl apply -f portainer-rbac.yaml #Create Cluster Role Binding
kubectl apply -f portainer-sa.yaml #Create Service Account
kubectl apply -f portainer-depl.yaml #Deploy Portainer Application
kubectl apply -f portainer-svc.yaml #Expose Portainer application using a LB
```
You can deploy all at once as follows:

   ```
   cd Portainer/
   kubectl apply -f .
   ```
Wait for the LoadBalancer service to obtain an external IP address. Check the status with:

```
kubectl get svc -n portainer
```
Once the external IP is assigned, you can access Portainer at `http://EXTERNAL_IP:9000`.
### Clean Up

To remove the Dashy deployment and associated resources, run the following commands:

```
kubectl delete -f dashy-svc.yaml
kubectl delete -f dashy-depl.yaml
kubectl delete -f dashy-pvc.yaml
kubectl delete -f dashy-pv.yaml
kubectl delete -f dashy-namespace.yaml
```
You can delete all at once as follows:
   ```
   cd Dashy/
   kubectl delete -f .
   ```