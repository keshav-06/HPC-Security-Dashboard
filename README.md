# HPC-Security-Dashboard
'HPE CTY Program Project'

## Objective:
In this we were asked to make a Security Dashboard for vatious Kubernetes Services.


In this project I have installed Docker, Kubernetes and Cilium using Virtual Machines.

- Used Docker Desktop for running containers.
- Installed K8s on openSUSE and Windows.
- Created Cluster Mesh using Microsoft Azure Services.
- Installed Cilium on K8s.
- Created Network Policies and implemented them.
- Created a **Grafana Dashboard** to visualise the results of the policies implemented.

### Docker Install:

#### Prerequisites

Before installing Docker, ensure that your system meets the following prerequisites
- Operating System: Linux, macOS, or Windows
- Architecture: x86_64 (64-bit) for Linux or macOS, or x86_64 or arm64 for Windows
- Kernel version: 3.10 or higher (Linux)
- RAM: At least 2GB

#### Installation Steps
Follow the steps below to install Docker on your system:

1. **Linux**: Open a terminal and execute the following commands:
   ```shell
   $ curl -fsSL https://get.docker.com -o get-docker.sh
   $ sudo sh get-docker.sh
   ```

2. **macOS**: Install Docker Desktop for macOS by following the instructions on the [Docker Webise](https://docs.docker.com/desktop/install/mac-install/).

3. **Windows**: Install Docker Desktop for Windows by following the instructions on the [Docker Website](https://docs.docker.com/desktop/install/windows-install/).

#### Verification
After the installation is complete, you can verify the Docker installation by running the following command in a terminal:
    ```shell
    $ docker --version
    ```

### Kubernetes Install:

#### Prerequisites

Before installing Kubernetes, ensure that your system meets the following prerequisites:
- Operating System: Linux, macOS, or Windows
- Processor Architecture: x86_64 (64-bit)
- RAM: Minimum 2GB

#### Installation Steps

Follow the steps below to install Kubernetes with Minikube on your system:

1. **Linux**:
   - For Linux, you can install Minikube using a package manager like apt, yum, or dnf. Open a terminal and execute the following commands:
     ```shell
     $ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
     $ sudo install minikube-linux-amd64 /usr/local/bin/minikube
     ```

2. **macOS**:
   - On macOS, you can use Homebrew to install Minikube. Open a terminal and execute the following command:
     ```shell
     $ brew install minikube
     ```

3. **Windows**:
   - On Windows, you can use Chocolatey to install Minikube. Open an elevated PowerShell or Command Prompt and execute the following command:
     ```shell
     $ choco install minikube
     ```

#### Starting Minikube

After the installation is complete, you can start Minikube by running the following command in a terminal or command prompt:

    ```shell
    $ minikube start
    ```
#### Verification
To verify the Minikube installation and check the status of the cluster, run the following command:

    ```shell
    $ kubectl cluster-info
    ```
    
For more information on using Minikube and Kubernetes, refer to the [official Minikube documentation](https://minikube.sigs.k8s.io/docs/) and [official Kubernetes documentation](https://kubernetes.io/docs/home/).


### Setting Up Grafana Dashboard

#### Prerequisites

Before setting up Grafana, ensure that you have the following prerequisites:

- Operating System: Linux, macOS, or Windows
- Docker: Grafana can be run as a Docker container, so make sure Docker is installed on your system.

#### Installation Steps

Follow the steps below to set up Grafana dashboard:

1. **Step 1: Start Grafana Container**:

   Run the following command in a terminal or command prompt to start a Grafana container:

   ```shell
   $ docker run -d -p 3000:3000 --name grafana grafana/grafana
   ```
   
   This command will download the Grafana Docker image (if not already available) and start a Grafana container on port 3000.

2. **Step 2: Access Grafana Dashboard:**

  Open a web browser and navigate to http://localhost:3000 to access the Grafana dashboard.

3. **Step 3: Log in to Grafana:**

    Use the default login credentials to log in to Grafana:

    Username: admin
    Password: admin
    
4. **Step 4: Configure Data Source:**

 - Once logged in, click on the "Configuration" gear icon on the left sidebar, then select "Data Sources".
  Click on "Add data source" and choose the type of data source you want to configure (e.g., Prometheus, InfluxDB, etc.).
 - Fill in the necessary details for your data source, such as the URL, authentication, and other settings.
 - Click "Save & Test" to verify the connection to your data source.

5. **Step 5: Create a Dashboard:**

 - After configuring the data source, click on the "+" icon on the left sidebar, then select "Dashboard" > "New Dashboard".
 - Customize your dashboard by adding panels, visualizations, and queries based on your data source.
 - Save your dashboard and give it a meaningful name.
For more information and advanced usage of Grafana, refer to the [official Grafana documentation](https://grafana.com/docs/).

### Cilium Installation with Hubble
![Cilium](https://avatars.githubusercontent.com/u/21054566?s=280&v=4)

#### Prerequisites

Before installing Cilium with Hubble, ensure that your system meets the following prerequisites:

- Operating System: Linux (Kernel version 4.9 or higher)
- Kubernetes: Cilium requires a Kubernetes cluster

#### Installation Steps

Follow the steps below to install Cilium with Hubble on your system:

1. **Step 1: Install Cilium**:

   - Install Cilium on your Kubernetes cluster using the following command:
     ```shell
     $ kubectl create -f https://raw.githubusercontent.com/cilium/cilium/v1.10/install/kubernetes/quick-hubble-install.yaml
     ```

2. **Step 2: Verify Cilium Installation**:

   - Wait for the Cilium installation to complete. You can check the status by running the following command:
     ```shell
     $ kubectl get pods -n kube-system --selector=k8s-app=cilium
     ```

   - Ensure that all the Cilium pods are running and ready before proceeding.

3. **Step 3: Install Hubble**:

   - Install Hubble using the following command:
     ```shell
     $ kubectl apply -f https://raw.githubusercontent.com/cilium/hubble/master/install/kubernetes-hubble.yaml
     ```

4. **Step 4: Verify Hubble Installation**:

   - Wait for the Hubble installation to complete. You can check the status by running the following command:
     ```shell
     $ kubectl get pods -n kube-system --selector=k8s-app=hubble
     ```

   - Ensure that all the Hubble pods are running and ready before proceeding.

5. **Step 5: Access Hubble UI**:

   - To access the Hubble UI, run the following command to start a port-forward to the Hubble UI service:
     ```shell
     $ kubectl port-forward -n kube-system service/hubble-ui 8080:80
     ```

   - Open a web browser and navigate to `http://localhost:8080` to access the Hubble UI.


For more information on Cilium and Hubble, refer to the [official Cilium documentation](https://docs.cilium.io/) and [official Hubble documentation](https://github.com/cilium/hubble).

#### Cilium Architecture

![Cilium Architecture](https://www.solo.io/wp-content/uploads/2023/01/Cilium-architecture-and-components.png)

### Policy Making in Cilium

Cilium provides powerful network security and policy enforcement capabilities within Kubernetes. This guide will walk you through the process of creating and applying network policies using Cilium.

#### Prerequisites

Before creating policies in Cilium, ensure that you have:

- Installed and configured Cilium on your Kubernetes cluster.
- Basic knowledge of Kubernetes concepts and networking.

#### Creating Network Policies

Follow the steps below to create network policies in Cilium:

1. **Step 1: Define Policy Rules**:

   - Determine the communication rules you want to enforce within your Kubernetes cluster. For example, you may want to allow traffic from a specific set of pods to a particular service, while blocking all other traffic.

   - Network policies in Cilium use the Kubernetes NetworkPolicy resource. Define the desired policy rules in a YAML file, specifying the source and destination selectors, ports, and protocols.

   - Here's an example of a simple network policy YAML file:

     ```yaml
     apiVersion: networking.k8s.io/v1
     kind: NetworkPolicy
     metadata:
       name: allow-frontend-to-backend
     spec:
       podSelector:
         matchLabels:
           app: frontend
       ingress:
       - from:
         - podSelector:
             matchLabels:
               app: backend
         ports:
         - protocol: TCP
           port: 80
     ```

2. **Step 2: Apply the Network Policy**:

   - Apply the network policy by running the following command:

     ```shell
     $ kubectl apply -f network-policy.yaml
     ```

   - Replace `network-policy.yaml` with the actual filename or path to your network policy YAML file.

3. **Step 3: Verify the Policy**:

   - To verify that the network policy has been applied successfully, run the following command:

     ```shell
     $ kubectl get networkpolicies
     ```

   - This command will display the list of network policies in your Kubernetes cluster.

   - You can also check the Cilium status to ensure that the policies are correctly enforced:

     ```shell
     $ cilium status
     ```

   - If the policies are applied and enforced without errors, the status output will confirm their successful implementation.

#### Updating and Deleting Network Policies

To update or delete a network policy in Cilium, follow these steps:

- **Updating a Network Policy**:
  - Make the necessary changes to the network policy YAML file.
  - Run the following command to update the policy:
    ```shell
    $ kubectl apply -f network-policy.yaml
    ```

- **Deleting a Network Policy**:
  - To delete a network policy, run the following command:
    ```shell
    $ kubectl delete networkpolicy <policy-name>
    ```

  - Replace `<policy-name>` with the name of the network policy you want to delete.


For more advanced usage and configuration options, refer to the [official Cilium documentation](https://docs.cilium.io/).





