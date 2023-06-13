## Deploying Cilium on Minikube for Basic Testing:
To get familiar with Cilium easily you can follow the Cilium Kubernetes Getting Started Guide to perform a basic DaemonSet installation of Cilium in minikube.

# To start minikube, minimal version required is >= v1.5.2, run the with the following arguments:

minikube version

minikube version: v1.5.2

minikube start --network-plugin=cni

# For minikube you can install Cilium using its CLI tool. To do so, first download the latest version of the CLI with the following command:

curl -LO https://github.com/cilium/cilium-cli/releases/latest/download/cilium-linux-amd64.tar.gz

# Then extract the downloaded file to your /usr/local/bin directory with the following command:

 sudo tar xzvfC cilium-linux-amd64.tar.gz /usr/local/bin
 
 rm cilium-linux-amd64.tar.gz
 
# After running the above commands, you can now install Cilium with the following command:

cilium install

# Cilium will then automatically detect the cluster configuration and create and install the appropriate components for a successful installation. The components are:

Certificate Authority (CA) in Secret cilium-ca and certificates for Hubble (Cilium's observability layer).
1. Service accounts.
2. Cluster roles.
3. ConfigMap.
4. Agent DaemonSet and an Operator Deployment.

After the installation, you can view the overall status of the Cilium deployment with the cilium status command.

# Understanding Cilium components
Deploying a cluster with Cilium adds Pods to the kube-system namespace. To see this list of Pods run:

kubectl get pods --namespace=kube-system -l k8s-app=cilium

# You'll see a list of Pods similar to this:

NAME           READY   STATUS    RESTARTS   AGE
cilium-kkdhz   1/1     Running   0          3m23s
...

A cilium Pod runs on each node in your cluster and enforces network policy on the traffic to/from Pods on that node using Linux BPF

Cilium provides a command-line interface (CLI) called cilium that you can use to manage and interact with Cilium in your Kubernetes cluster. Here are some commonly used cilium commands:

# View Cilium status:
    cilium status
This command displays the status of Cilium components in your cluster, including the version, number of nodes, health status, and any detected issues.

# Check Cilium connectivity:
    cilium connectivity check
This command verifies the connectivity between Cilium agents on different nodes in the cluster. It ensures that the networking infrastructure is properly set up and functioning correctly.

# View Cilium endpoints:
    cilium endpoint list
This command shows the list of Cilium endpoints (pods or services) in your cluster. It provides information about the IP addresses, namespaces, labels, and other details of the endpoints.

# Manage Cilium network policies:
    cilium network-policy
Use this command to manage Cilium network policies, which define how traffic should flow between different endpoints in your cluster. You can create, view, update, and delete network policies using subcommands like create, get, update, and delete.

# Manage Cilium cluster-wide network policies:
    cilium cluster-wide-network-policy
This command allows you to manage Cilium cluster-wide network policies, which apply to traffic across all namespaces in your cluster. Similar to network policies, you can use subcommands like create, get, update, and delete to manage cluster-wide network policies.

# Debug Cilium:
    cilium debug
Use this command to gather diagnostic information and perform troubleshooting tasks for Cilium. It provides subcommands for collecting logs, running connectivity diagnostics, and more.


#### Configure IPAM (IP Address Management):

# To view the IPAM configuration:

cilium ipam show

# To configure IPAM with a specific IP range:

cilium ipam create --range <CIDR>
Replace <CIDR> with the desired IP range in CIDR notation, such as 10.0.0.0/16.

# To delete the IPAM configuration:
cilium ipam delete

## Configure BPF mode:
# To view the current BPF (Berkeley Packet Filter) mode:
cilium bpf mode show

# To switch to a specific BPF mode (e.g., auto, always, or off):
cilium bpf mode <mode>

# Replace <mode> with the desired mode, such as auto, always, or off.
Configure tunneling:

# To view the current tunneling configuration:
cilium tunnel show

# To enable VXLAN tunneling mode:
cilium tunnel vxlan enable

# To enable Geneve tunneling mode:
cilium tunnel geneve enable

# To disable tunneling:
cilium tunnel disable

## Configure cluster mesh:
# To enable cluster mesh (inter-node communication):
cilium cluster mesh enable

# To disable cluster mesh:
cilium cluster mesh disable

These are some of the commands available to configure networking aspects in Cilium using the cilium CLI. Keep in mind that the specific options and flags may vary depending on the version of Cilium you are using. You can refer to the official Cilium documentation or use the cilium help command to explore additional commands and options for network configuration in Cilium.

