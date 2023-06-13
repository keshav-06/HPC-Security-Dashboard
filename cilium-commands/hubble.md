# In order to enable Hubble, run the command cilium hubble enable as shown below:
 cilium hubble enable
🔑 Found existing CA in secret cilium-ca
✨ Patching ConfigMap cilium-config to enable Hubble...
♻️  Restarted Cilium pods
🔑 Generating certificates for Relay...
2021/04/13 17:11:23 [INFO] generate received request
2021/04/13 17:11:23 [INFO] received CSR
2021/04/13 17:11:23 [INFO] generating key: ecdsa-256
2021/04/13 17:11:23 [INFO] encoded CSR
2021/04/13 17:11:23 [INFO] signed certificate with serial number 365589302067830033295858933512588007090526050046
2021/04/13 17:11:24 [INFO] generate received request
2021/04/13 17:11:24 [INFO] received CSR
2021/04/13 17:11:24 [INFO] generating key: ecdsa-256
2021/04/13 17:11:24 [INFO] encoded CSR
2021/04/13 17:11:24 [INFO] signed certificate with serial number 644167683731852948186644541769558498727586273511
✨ Deploying Relay...

# Run cilium status to validate that Hubble is enabled and running:
 cilium status
    /¯¯\
 /¯¯\__/¯¯\    Cilium:         OK
 \__/¯¯\__/    Operator:       OK
 /¯¯\__/¯¯\    Hubble:         OK
 \__/¯¯\__/    ClusterMesh:    disabled
    \__/

DaemonSet         cilium                   Desired: 3, Ready: 3/3, Available: 3/3
Deployment        cilium-operator          Desired: 1, Ready: 1/1, Available: 1/1
Deployment        hubble-relay             Desired: 1, Ready: 1/1, Available: 1/1
Containers:       cilium                   Running: 3
                  cilium-operator          Running: 1
                  hubble-relay             Running: 1
Image versions    cilium-operator          quay.io/cilium/operator-generic:v1.9.5: 1
                  hubble-relay             quay.io/cilium/hubble-relay:v1.9.5: 1
                  cilium                   quay.io/cilium/cilium:v1.9.5: 3

# Download the latest hubble release:
curl -LO "https://raw.githubusercontent.com/cilium/hubble/master/stable.txt"
set /p HUBBLE_VERSION=<stable.txt
curl -LO "https://github.com/cilium/hubble/releases/download/%HUBBLE_VERSION%/hubble-windows-amd64.tar.gz"
curl -LO "https://github.com/cilium/hubble/releases/download/%HUBBLE_VERSION%/hubble-windows-amd64.tar.gz.sha256sum"
certutil -hashfile hubble-windows-amd64.tar.gz SHA256
type hubble-windows-amd64.tar.gz.sha256sum
:: verify that the checksum from the two commands above match
tar zxf hubble-windows-amd64.tar.gz

# Validate Hubble API Access
In order to access the Hubble API, create a port forward to the Hubble service from your local machine. This will allow you to connect the Hubble client to the local port 4245 and access the Hubble Relay service in your Kubernetes cluster.

 cilium hubble port-forward&
Forwarding from 0.0.0.0:4245 -> 4245
Forwarding from [::]:4245 -> 4245

# Now you can validate that you can access the Hubble API via the installed CLI:

hubble status
Healthcheck (via localhost:4245): Ok
Current/Max Flows: 11917/12288 (96.98%)
Flows/s: 11.74
Connected Nodes: 3/3
You can also query the flow API and look for flows:

hubble observe

