## Set Up ArgoCD
#### Step 1: Create a Namespace for ArgoCD

To ensure ArgoCD has its own isolated environment within your Kubernetes cluster, create a dedicated namespace.

```bash
kubectl create ns argocd
```

---

#### Step 2: Install ArgoCD

Use the official installation manifest from ArgoCD’s GitHub repository to deploy it to your cluster.

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

This command installs all required ArgoCD components in the `argocd` namespace.

---

#### Step 3: Install ArgoCD CLI

To interact with the ArgoCD server from your local machine or a terminal, install the ArgoCD command-line interface (CLI).

```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```

Once installed, verify the installation using:

```bash
argocd version
```

---

#### Step 4: Check ArgoCD Services

To confirm that ArgoCD services are running:

```bash
kubectl get svc -n argocd
```

This lists all services in the `argocd` namespace. Take note of the `argocd-server` service, as it will be exposed in the next step.

---

#### Step 5: Expose ArgoCD Server Using NodePort

By default, the `argocd-server` service is of type `ClusterIP`, which makes it accessible only within the cluster. Change it to `NodePort` to expose it externally.

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec":{"type": "NodePort"}}'
```

Retrieve the updated service information to identify the assigned NodePort:

```bash
kubectl get svc -n argocd
```

Note the port in the `PORT(S)` column (e.g., `30529`).

---

#### Step 6: Configure AWS Inbound Rule for NodePort

If your Kubernetes cluster is hosted on AWS, ensure that the assigned NodePort is accessible by adding an inbound rule to your security group. Allow traffic on this port from the internet to the worker node(s).

---

#### Step 7: Access ArgoCD Web UI

With the NodePort and the worker node’s public IP, access the ArgoCD web UI:

```bash
http://<worker-node-public-ip>:<node-port>
```

![image](https://github.com/user-attachments/assets/32222a1f-3aea-450b-a7e5-0f44b34702ed)


For the initial login:

* **Username:** `admin`
    
* **Password:** Retrieve using the following command:
    

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```

Change the password after logging in by navigating to the user info section in the ArgoCD UI.

---

#### Step 8: Log In to ArgoCD via CLI

To log in from the CLI, use the public IP and NodePort:

```bash
argocd login <worker-node-public-ip>:<node-port> --username admin
```

For example:

```bash
argocd login 54.154.41.147:30529 --username admin
```

---

#### Step 9: Check ArgoCD Cluster Configuration

To view the cluster configurations managed by ArgoCD:

```bash
argocd cluster list
```

---

#### Step 10: Add a Cluster to ArgoCD

If your cluster is not already added, first identify its context:

```bash
kubectl config get-contexts
```

Then, add the desired cluster to ArgoCD. Replace the placeholders with your actual cluster context and name:

```bash
argocd cluster add <kube-context> --name <friendly-name>
```

For example:

```bash
argocd cluster add mega-project-user@bankapp-cluster.eu-west-1.eksctl.io --name bankapp-cluster
```

#### Step 11: Add Project Repository in ArgoCD UI

To integrate your Git repository with ArgoCD:

1. Navigate to **Settings** &gt; **Repositories** in the ArgoCD UI.
    
2. Click on **Connect Repo** and provide the appropriate repository URL.
    
3. Select the connection method as HTTPS. If the repository is private:
    
    * Enter your username and password to authenticate.
        
    * Otherwise, skip the authentication step for public repositories.
        
4. Choose the default project (or any specific project, if configured) and complete the setup.
    

Once connected, your repository will be ready for deploying applications via ArgoCD.

---
# Step 10: Installing Helm, Ingress Controller, and Setting Up Metrics for HPA in Kubernetes

### 1\. Install Helm

**Helm** is a powerful Kubernetes package manager that simplifies the deployment and management of applications within your Kubernetes clusters. To get started, follow the steps below to install Helm on your local system:

```bash
# Download the Helm installation script
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3

# Change script permissions to make it executable
chmod 700 get_helm.sh

# Run the installation script
./get_helm.sh
```

After running the script, Helm will be installed, and you can start using it to deploy applications to your Kubernetes cluster.

---

### 2\. Install Ingress Controller Using Helm

An **Ingress Controller** is necessary to manage external HTTP/HTTPS access to your services in Kubernetes. In this step, we will install the NGINX Ingress Controller using Helm.

To install the NGINX Ingress Controller, execute the following commands:

```bash
# Add the NGINX Ingress controller Helm repository
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

# Update the Helm repository to ensure you have the latest charts
helm repo update

# Install the ingress-nginx controller in the ingress-nginx namespace
helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx --create-namespace
```

This command installs the NGINX Ingress controller into your Kubernetes cluster, creating a new namespace called `ingress-nginx`. This Ingress controller will handle routing and load balancing for your services.

---

### 3\. Apply Metrics Server for HPA

To enable **Horizontal Pod Autoscaling (HPA)** in your Kubernetes cluster, the **metrics-server** is required to collect resource usage data like CPU and memory from the pods. HPA scales your application based on these metrics.

Run the following command to apply the **metrics-server**:

```bash
# Install metrics-server to collect resource usage metrics
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Once installed, the metrics-server will start collecting data from your Kubernetes nodes and pods, enabling you to configure HPA based on these metrics.

---

### 4\. Install Cert-Manager for SSL/TLS Certificates

For securing your application with **HTTPS** using your custom domain name, you need to generate SSL/TLS certificates. **Cert-Manager** is a Kubernetes tool that automates the management and issuance of these certificates.

To install Cert-Manager, use the following command:

```bash
# Apply Cert-Manager components to your cluster
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.16.2/cert-manager.yaml
```

Once installed, Cert-Manager will be responsible for automatically issuing and renewing SSL/TLS certificates for your services. You can then configure Cert-Manager to issue a certificate for your application and configure HTTPS with your domain.

---

## Step 11: Creating an Application on ArgoCD

### 1\. General Section

* **Application Name**: Choose a name for your application.
    
* **Project Name**: Select **default**.
    
* **Sync Policy**: Choose **Automatic**.
    
* Enable **Prune Resources** and **Self-Heal**.
    
* Check **Auto Create Namespace**.
    

---

### 2\. Source Section

* **Repo URL**: Enter the URL of your Git repository.
    
* **Revision**: Select the branch (e.g., `main`).
    
* **Path**: Specify the directory containing your Kubernetes manifests (e.g., `k8s`).
    

---

### 3\. Destination Section

* **Cluster**: Select your desired cluster.
    
* **Namespace**: Use `bankapp-namespace`.
    

---