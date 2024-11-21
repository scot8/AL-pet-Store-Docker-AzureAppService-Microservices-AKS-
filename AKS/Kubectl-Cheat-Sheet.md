# Kubernetes Cheat Sheet

## Basic Kubernetes Commands

### 1. Cluster and Node Management

- **kubectl get nodes**  
  Lists all nodes in the cluster with their status.

- **kubectl describe node `node-name`**  
  Shows detailed information about a specific node.

### 2. Working with Pods

- **kubectl get pods**  
  Lists all pods in the current namespace.

- **kubectl get pods -A**  
  Lists all pods across all namespaces.

- **kubectl describe pod `pod-name**  
  Shows detailed information about a specific pod, including events and container details.

- **kubectl logs `pod-name`**  
  Displays the logs from a specific pod. Useful for debugging applications.

- **kubectl exec -it `pod-name` -- `command`**  
  Runs a command inside a running container (useful for debugging). Example: kubectl exec -it pod-name -- /bin/bash.

### 3. Working with Deployments

- **kubectl get deployments**  
  Lists all deployments in the current namespace.

- **kubectl describe deployment `deployment-name`**  
  Shows detailed information about a specific deployment.

- **kubectl scale deployment `deployment-name` --replicas=`number`**  
  Scales the specified deployment to the desired number of replicas.

- **kubectl rollout restart deployment 'deployment-name'**  
  Restarts all pods in a deployment with zero downtime.

- **kubectl delete deployment 'deployment-name'**  
  Deletes a specific deployment.

### 4. Working with Services

- **kubectl get svc**  
  Lists all services in the current namespace.

- **kubectl describe svc 'service-name'**  
  Shows detailed information about a specific service.

- **kubectl expose deployment 'deployment-name' --type='type' --port='port'**  
  Exposes a deployment as a service of specified type (ClusterIP, NodePort, or LoadBalancer).

- **kubectl delete svc 'service-name'**  
  Deletes a specific service.

### 5. Configurations and Secrets

- **kubectl get configmaps**  
  Lists all ConfigMaps in the current namespace.

- **kubectl describe configmap 'configmap-name'**  
  Shows detailed information about a specific ConfigMap.

- **kubectl get secrets**  
  Lists all secrets in the current namespace.

- **kubectl describe secret 'secret-name'**  
  Shows detailed information about a specific secret.

### 6. Namespace Management

- **kubectl get namespaces**  
  Lists all namespaces in the cluster.

- **kubectl create namespace 'namespace-name'**  
  Creates a new namespace.

- **kubectl delete namespace 'namespace-name'**  
  Deletes a specific namespace.

### 7. Apply and Delete Configurations

- **kubectl apply -f 'file-name.yaml'**  
  Creates or updates resources defined in the YAML file.

- **kubectl delete -f 'file-name.yaml'**  
  Deletes resources defined in the YAML file.

### 8. Other Helpful Commands

- **kubectl get all**  
  Lists all resources (pods, services, deployments, etc.) in the current namespace.

- **kubectl get events**  
  Shows events from the cluster, useful for debugging issues.

- **kubectl top nodes**  
  Displays CPU and memory usage of all nodes.

- **kubectl top pods**  
  Displays CPU and memory usage of all pods in the current namespace.
---

This cheat sheet provides essential commands to help you navigate Kubernetes and perform fundamental tasks like deployment, scaling, and service exposure.
