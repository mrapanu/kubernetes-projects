# Kubernetes Projects

This repository contains the Kubernetes configuration files for various projects. Below is the documentation for each project:

* [Dashy](#dashy)
* [Portainer](#portainer)
* [Monitoring](#Monitoring)
   * [Prometheus](#Prometheus)
   * [Prometheus-Operator](#Prometheus-Operator)
   * [Node-Exporter](#Node-Exporter)
   * [Grafana](#Grafana)
   * [Alert Manager](#AlertManager) 

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

### Prometheus-Operator

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

### Node-Exporter

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

[Grafana](https://grafana.com/) is an open-source analytics and monitoring platform that integrates with various data sources, allowing you to visualize and understand your metrics and logs better. It provides a flexible and interactive dashboard for monitoring, alerting, and exploring data.

#### Contents

1. [grafana-configmap.yaml](grafana/grafana-configmap.yaml)
    - ConfigMap definition for Grafana settings.

2. [grafana-depl.yaml](grafana/grafana-depl.yaml)
    - Deployment definition for Grafana.

3. [grafana-pv.yaml](grafana/grafana-pv.yaml)
    - Persistent Volume definition for Grafana storage.

4. [grafana-pvc.yaml](grafana/grafana-pvc.yaml)
    - Persistent Volume Claim definition for Grafana storage.

5. [grafana-sa.yaml](grafana/grafana-sa.yaml)
    - Service Account definition for Grafana.

6. [grafana-secret.yaml](grafana/grafana-secret.yaml)
    - Secret definition for storing sensitive information (admin username and password).
    - **IMPORTANT**: The `admin-user` and `admin-password` fields are required and should be encoded using the following commands:
        ```
        echo username_example -n | base64
        echo username_passwd -n | base64
        ```

7. [grafana-svc.yaml](grafana/grafana-svc.yaml)
    - Service definition for exposing Grafana via a LoadBalancer.

8. [grafana-svcmonitor.yaml](grafana/grafana-svcmonitor.yaml)
    - ServiceMonitor definition for Prometheus monitoring integration.

#### Deploy

To deploy Grafana in your Kubernetes cluster, use the following commands:

```
kubectl apply -f grafana/grafana-configmap.yaml
kubectl apply -f grafana/grafana-depl.yaml
kubectl apply -f grafana/grafana-pv.yaml
kubectl apply -f grafana/grafana-pvc.yaml
kubectl apply -f grafana/grafana-sa.yaml
kubectl apply -f grafana/grafana-secret.yaml
kubectl apply -f grafana/grafana-svc.yaml
kubectl apply -f grafana/grafana-svcmonitor.yaml
```

You can deploy all at once as follows:

   ```
   cd Monitoring/grafana
   kubectl apply -f .
   ```

#### Setup Grafana in Web Console
To set up Grafana in the web console, follow these steps:

Access the Grafana web console using the provided LoadBalancer IP and port (default: 3000).

Log in with the credentials configured in the grafana-secret.yaml Secret.

After login, first thing to do is to configure the datasource. So go to Configuration -> Datasources, then hit "Add data source". From Time series databases select "Prometheus".
`IMPORTANT`: You must have prometheus up and running in your kubernetes cluster. Below you have an example:

![Grafana DS Configuration](https://github.com/mrapanu/kubernetes-projects/blob/main/images/grafana_datasource.png?raw=true)

Note: URL is http://<prometheus pod name following by . namespace : prometheus_port>

Example for monitoring all nodes. From Dashboards select Manage, then hit "Import" button. We will import an existing dashboard from
[grafana dashboards](https://grafana.com/grafana/dashboards/). For example choose this [dashboard](https://grafana.com/grafana/dashboards/11074-node-exporter-for-prometheus-dashboard-en-v20201010/). 

`IMPORTANT`: You must have node-exporter up and running in your kubernetes cluster.

Copy the ID of the dashboard (this example: 11074) and hit "Load" button. Now the dashboard is available and can be accessed from Dashboards->Manage->Name of the dashboard (Example here: Node Monitor). Below you have an example:

![Grafana Node Monitor Dashboard](https://grafana.com/api/dashboards/11074/images/8427/image)

#### Cleanup

To delete the Grafana project and associated resources, use the following commands:

   ```
   cd Monitoring/grafana
   kubectl delete -f .
   ```

### AlertManager

#### Contents

1. **alertmanager/alertmanager-config.yaml**
    - **Description:** Defines the Alertmanager configuration with specific settings for integrating with RocketChat. It includes configurations for routing, grouping alerts, and specifying receivers with webhook configurations.

2. **alertmanager/alertmanager-depl.yaml**
    - **Description:** Specifies the deployment details for Alertmanager in the Kubernetes cluster. It includes version, replicas, and resource allocation for the Alertmanager instance.

3. **alertmanager/alertmanager-rules.yaml**
    - **Description:** Contains custom alerting rules for Alertmanager. These rules address potential issues in the Kubernetes cluster, such as instance downtime, high CPU temperature, high CPU usage, and high Memory usage.

4. **alertmanager/alertmanager-svcmonitor.yaml**
    - **Description:** Defines the monitoring settings for the Alertmanager service. It specifies the endpoints, namespace selection, and selector labels for monitoring the Alertmanager instance.

#### Deploy

To deploy AlertManager in your Kubernetes cluster, use the following commands:

```
kubectl apply -f alertmanager/alertmanager-config.yaml
kubectl apply -f alertmanager/alertmanager-depl.yaml
kubectl apply -f alertmanager/alertmanager-rules.yaml
kubectl apply -f alertmanager/alertmanager-svcmonitor.yaml
```

You can deploy all at once as follows:

   ```
   cd Monitoring/alertmanager
   kubectl apply -f .
   ```
#### Cleanup

To delete the AlertManager project and associated resources, use the following commands:

   ```
   cd Monitoring/alertmanager
   kubectl delete -f .
   ```