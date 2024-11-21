# Lab 6 - CST8915 Full-stack Cloud-native Development: Deploy Algonquin Pet Store to Azure Kubernetes Service (AKS)

Welcome to Lab 6 of the **CST8915 Full-stack Cloud-native Development** course. In this lab, you will deploy the [**Algonquin Pet Store**](https://github.com/ramymohamed10/algonquin-pet-store) application to **Azure Kubernetes Service (AKS)**. You will create an AKS cluster, deploy the application using a provided YAML file, and expose the services for external access.

> **Note:** This lab will incur an approximate cost of $3 to your Azure credit. Be sure to clean up your resources after completing the lab to avoid additional charges.


---
### What is Azure Kubernetes Service (AKS)?

[**Azure Kubernetes Service (AKS)**](https://learn.microsoft.com/en-us/azure/aks/what-is-aks) is a managed Kubernetes service provided by Microsoft Azure, designed to simplify the deployment, management, and scaling of containerized applications using Kubernetes. With AKS, Azure handles critical tasks such as provisioning, scaling, upgrading, and monitoring the Kubernetes cluster, allowing you to focus on deploying and managing your applications. AKS supports industry-standard Kubernetes, ensuring that the applications you build are portable and compatible across different environments.

Using AKS, you can:
- **Deploy and manage** applications with ease using Kubernetes’ declarative approach.
- **Scale applications** automatically, based on demand, with Kubernetes' built-in scaling mechanisms.
- **Monitor and secure** your clusters with integrated Azure security and monitoring tools.
- **Integrate with other Azure services**, such as networking, storage, and identity services, enhancing the capabilities of your applications.

---

In this lab, you’ll gain hands-on experience deploying microservices to AKS, connecting to the cluster using `kubectl`, and exposing the application to the internet.

---
## Step 1: Install `kubectl`

1. **What is `kubectl`?**
   - `kubectl` is a command-line tool that allows you to communicate with and manage Kubernetes clusters. You will use `kubectl` to deploy applications, configure clusters, and inspect resources.

2. **Installing `kubectl`:**
   - Follow the official installation guide to install `kubectl` on your system. The guide provides detailed instructions for various operating systems.
   - [kubectl Installation Guide](https://kubernetes.io/docs/tasks/tools/)

3. **Verify `kubectl` Installation:**
   - After installing, confirm that `kubectl` is properly set up by running:
     ```bash
     kubectl version --client
     ```
   - You should see the client version information displayed, confirming a successful installation.

---
## Step 2: Create an Azure Kubernetes Cluster (AKS)

1. **Log in to Azure Portal:**
   - Go to [https://portal.azure.com](https://portal.azure.com) and log in with your Azure account.

2. **Create a Resource Group:**
   - In the Azure Portal, search for **Resource Groups** in the search bar.
   - Click **Create** and fill in the following:
     - **Resource group name**: `AlgonquinPetStoreRG`
     - **Region**: `Canada`.
   - Click **Review + Create** and then **Create**.

3. **Create an AKS Cluster:**
   - In the search bar, type **Kubernetes services** and click on it.
   - Click **Create** and select **Kubernetes cluster**
   - In the `Basics` tap fill in the following details:
     - **Subscription**: Select your subscription.
     - **Resource group**: Choose `AlgonquinPetStoreRG`.
     - **Cluster preset configuration**: Choose `Dev/Test`.
     - **Kubernetes cluster name**: `AlgonquinPetStoreCluster`.
     - **Region**: Same as your resource group (e.g., `Canada`).
     - **Availability zones**: `None`.
     - **AKS pricing tier**: `Free`.
     - **Kubernetes version**: `Default`.
     - **Automatic upgrade**: `Disabled`.
     - **Automatic upgrade scheduler**: `No schedule`.
     - **Node security channel type**: `None`.
     - **Security channel scheduler**: `No schedule`.
     - **Authentication and Authorization**: `Local accounts with Kubernetes RBAC`.
   - In the `Node pools` tap fill in the following details:
     - Select **agentpool**. Optionally change its name to `masterpool`. This nodes will have the controlplane.
        - Set **node size** to `D2as_v4`.
        - **Scale method**: `Manual`
        - **Node count**: `1`
        - Click `update`
     - Click on **Add node pool**:
        - **Node pool name**: `workerspool`.
        - **Mode**: `User` 
        - Set **node size** to `D2as_v4`.
        - **Scale method**: `Manual`
        - **Node count**: `2`
        - Click `add`
   - Click **Review + Create**, and then **Create**. The deployment will take a few minutes.

4. **Connect to the AKS Cluster:**
   - Once the AKS cluster is deployed, navigate to the cluster in the Azure Portal.
   - In the overview page, click on **Connect**. 
   - Select **Azure CLI** tap. You will need Azure CLI. If you don't have it: [**Install Azure CLI**](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
   - Login to your azure account using the following command:
      ```
      az login
      ```
   - Set the cluster subscription using the command shown in the portal (it will look something like this):
      ```
      az account set --subscription 'subscribtion-id'
      ```

   - Copy the command shown in the portal for configuring `kubectl` (it will look something like this):
     ```
     az aks get-credentials --resource-group AlgonquinPetStoreRG --name AlgonquinPetStoreCluster
     ```
      **Understanding the Command:**
      - The command `az aks get-credentials` pulls the necessary configuration files to enable `kubectl` to access your AKS cluster. Here’s a breakdown:
     - `--resource-group` specifies the resource group where your AKS cluster resides.
     - `--name` specifies the name of your AKS cluster.
     - `--overwrite-existing` can be used to overwrite any existing Kubernetes configuration files for the same cluster. This is useful if you’ve connected to the cluster before or if multiple configurations exist for it.
   - Verify Cluster Access:
      - Test your connection to the AKS cluster by listing all nodes:
        ```
        kubectl get nodes
        ```
        You should see details of the nodes in your AKS cluster if the connection is successful.
---

## Step 3: Deploy the Algonquin Pet Store Application

1. **Apply the YAML file to the AKS cluster:**
   - In this step, use the K8s deployment YAML file provided: `algonquin-pet-store-all-in-one.yaml`.
   - Open the terminal and navigate to the file directory.
   - Run the following command to apply the YAML configuration and deploy the application to AKS:
      ```
      kubectl apply -f algonquin-pet-store-all-in-one.yaml
      ```

2. **Verify the deployment:**
   - After the command executes, verify that the pods are running by using the following command:
     ```bash
     kubectl get pods
     ```

3. **Check services:**
   - Confirm that all services are up and running:
     ```bash
     kubectl get services
     ```

4. **Access the Store Front Application:**
   - The **Store Front** service is configured as a LoadBalancer, which exposes the application to the internet.
   - In the Azure Portal, go to **Kubernetes Services** > **AlgonquinPetStoreCluster** > **Services and ingresses**.
   - Locate the **store-front** service, and note the **EXTERNAL-IP** address.

5. **Test the Store Front:**
   - Open a web browser and enter the external IP address to access the Store Front (You may need to wait about one minute).

6. **Verifying the backend services**:
   - `order-service` is accessible at:
      ```
      http://<EXTERNAL-IP>/orders
      ``` 
   - `product-service` is accessible at:
      ```
      http://<EXTERNAL-IP>/products
      ``` 
   - `RabbitMQ Management Dashboard` is accessible at:
      ```
      http://<EXTERNAL-IP>/rabbitmq
      ``` 
      - Use the following credentials to log in:
        - Username: myuser
        - Password: mypassword
7. Clean Up Kubernetes Resources:
- In this step, use the K8s deployment YAML file provided: `.
   - Open the terminal and navigate to the `algonquin-pet-store-all-in-one.yaml` file directory.
   - Run the following command to delete all resources defined in the YAML file.
      ```
      kubectl delete -f algonquin-pet-store-all-in-one.yaml
      ``` 
8. Clean Up Azure Resources:
   - Delete the Primary Resource Group (AlgonquinPetStoreRG)
   - Delete the Managed Cluster Resource Group (MC_AlgonquinPetStoreRG_AlgonquinPetStoreRG_canadacentral)
   - Delete the Monitoring Resource Group (MA_defaultazuremonitorworkspace-cca_canadacentral_managed)
   - Delete the Network Watcher Resource Group (NetworkWatcherRG):
---

## Lab Tasks: Updating API Endpoints
1. Read `kubectl` documentation: https://kubernetes.io/docs/reference/kubectl/
2. Test the essential `kubectl` commands in the provided `Kubectl-Cheat-Sheet.md` file.
3. The application’s various services (such as `product-service`, `order-service`, and `RabbitMQ Management Console`) are all accessible via the same IP address, differentiated by paths (`/products`, `/orders`, `/rabbitmq`). 
    - Figure out how this setup is possible.
    - `Store-front` was slightly modified to achieve that. Examine the updated `store-front` repo: https://github.com/ramymohamed10/store-front-L6.
    - **Hint**: start by investigating `nginx.conf` file. 


