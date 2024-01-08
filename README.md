# Kubernetes Projects

This repository contains the Kubernetes configuration files for various projects. Below is the documentation for each project:

1. [Dashy](#dashy)
2. [Portainer](#portainer)
3. [Monitoring](#Monitoring)
   3.1 [Prometheus](#Prometheus)
   3.2 [Prometheus Operator](#Prometheus Operator)
   3.3 [Node Exporter](#Node Exporter)
   3.3 [Grafana](#Grafana)
   3.4 [Alert Manager](#AlertManager) 

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
3. NFS server is available with the required shared path. `IMPORTANT`: Change portainer-pv.yaml server and path section with your needs.

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

To remove the Portainer deployment and associated resources, run the following commands:

```
kubectl delete -f portainer-svc.yaml
kubectl delete -f portainer-depl.yaml
kubectl delete -f portainer-pvc.yaml
kubectl delete -f portainer-pv.yaml
kubectl delete -f portainer-rbac.yaml 
kubectl delete -f portainer-sa.yaml 
kubectl delete -f portainer-namespace.yaml
```
You can delete all at once as follows:
   ```
   cd Portainer/
   kubectl delete -f .
   ```

## Monitoring
This project includes configurations for setting up monitoring in a Kubernetes environment using Prometheus, node-exporter, AlertManager, Grafana etc.

`IMPORTANT`: Before starting deploying prometheus, grafana, node exporter etc. ensure that you apply the following:
- Apply the namespace configuration:
```
kubectl apply -f monitoring-namespace.yaml
```
- Apply Custom Resource Definitions (CRDs)
```
kubectl apply -f crds/
```

### Prometheus

Prometheus is an open-source monitoring and alerting toolkit designed for reliability and scalability in modern, dynamic environments.

#### Deployment Steps

`Important`: Replace `<YOUR_NFS_ADDRESS>` and path with the actual NFS server address in `prometheus-pv.yaml` and your path:


```
kubectl apply -f prometheus-pv.yaml #Create Persistent Volume
kubectl apply -f prometheus-pvc.yaml #Create Persistent Volume Claim
kubectl apply -f prometheus-crb.yaml #Create Cluster Role Binding
kubectl apply -f prometheus-cr.yaml #Create Cluster Role 
kubectl apply -f prometheus-sa.yaml #Create Service Account
kubectl apply -f prometheus-depl.yaml #Deploy Prometheus Application
kubectl apply -f prometheus-svc.yaml #Create Service for Prometheus
kubectl apply -f prometheus-svcmonitor.yaml #Create Service Monitor
```
You can deploy all at once as follows:

   ```
   cd Monitoring/prometheus
   kubectl apply -f .
   ```
#### Clean Up

   ```
   cd Monitoring/prometheus
   kubectl delete -f .
   ```

### Prometheus Operator

The Prometheus Operator is an open-source project that simplifies the deployment and management of Prometheus and related components in a Kubernetes environment.

#### Deployment Steps

```
kubectl apply -f promoperator-crb.yaml #Create Cluster Role Binding
kubectl apply -f promoperator-cr.yaml #Create Cluster Role 
kubectl apply -f promoperator-sa.yaml #Create Service Account
kubectl apply -f promoperator-depl.yaml #Deploy Prometheus Operator Application
kubectl apply -f promoperator-svc.yaml #Create Service for Prometheus Operator
kubectl apply -f promoperator-svcmonitor.yaml #Create Service Monitor
```
You can deploy all at once as follows:

   ```
   cd Monitoring/operator
   kubectl apply -f .
   ```
#### Clean Up

   ```
   cd Monitoring/operator
   kubectl delete -f .
   ```

### Node Exporter

The Node Exporter is a Prometheus exporter specifically designed to collect and expose various system-level metrics from a target host machine. It serves as an agent that runs on the machine being monitored, and its primary purpose is to gather detailed information about the system's health, performance, and resource utilization. These metrics can then be scraped and stored by a Prometheus server for monitoring and analysis.

#### Deployment Steps

```
kubectl apply -f node_exporter-crb.yaml #Create Cluster Role Binding
kubectl apply -f node_exporter-cr.yaml #Create Cluster Role 
kubectl apply -f node_exporter-sa.yaml #Create Service Account
kubectl apply -f node_exporter-dset.yaml #Create DaemonSet for node-exporter
kubectl apply -f node_exporter-svc.yaml #Create Service for 
kubectl apply -f node_exporter-smonitor.yaml #Create Service Monitor
```
You can deploy all at once as follows:

   ```
   cd Monitoring/node-exporter
   kubectl apply -f .
   ```
#### Clean Up

   ```
   cd Monitoring/node-exporter
   kubectl delete -f .
   ```

### Grafana
TO DO

### AlertManager
TO DO