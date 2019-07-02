# Prototype of running VS Code Extension as a remote plugin in Eclipse Che
This section explains how to run a simple VS code extension as a remote plugin in Eclipse Che

## Steps to Launch Eclipse Che

Below are the requirements for deploying multi-user deployment of Eclipse Che on Kubernetes on Debian-based Linux distributions. 

**A) Before launching Che on Minkube, we need to ensure that below requirements are fulfilled.** 

**1. Kubectl (installing on Linux OS)** 

- _Download_ the latest release with the command:
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl 
```
- Make the kubectl binary executable
```
chmod +x ./kubectl  
```

- Move the binary in to your PATH
```
sudo mv ./kubectl /usr/local/bin/kubectl   
```

- Now, you can check the version of kubectl installed 
```
kubectl version 
```
**2. Hypervisor: KVM or VirtualBox**

Install VrtualBox for Linux from https://www.virtualbox.org/wiki/Linux_Downloads

**3. Minikube**
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.1.0/minikube-linux-amd64 && chmod +x minikube && sudo cp minikube /usr/local/bin/ && rm minikube  
```

**4. Chectl**

- Download the binary and then navigate to the location where you have downloaded the binary.

- Make the checktl binary executable
```
chmod +x ./chectl-linux
```

- Move the binary in to your PATH
```
sudo mv ./chectl-linux /usr/local/bin/chectl
```
**B) After fulfilling the requirements, we can start Kubernetes Cluster.**

Start Kubernetes cluster with at least 10 GB RAM and RBAC:
```
minikube start --vm-driver=kvm2 --extra-config=apiserver.authorization-mode=RBAC --cpus 4 --memory 10240
```

**C) We can start Che server using chectl where you can pass the below command in the terminal.**
```
chectl server:start 
```

<img src="https://github.com/Rijul5/vscode-extension-che/raw/master/figs/che_server.png" width="700" height="400" alt="Che Server">


**D) Open the Che server using the URL generated when you launch Che server using Chectl**

Che Server URL...http://che-che.192.168.99.100.nip.io

<img src="https://github.com/Rijul5/vscode-extension-che/raw/master/figs/che_dash.png" width="700" height="400" alt="Che Dashboard">

**E) Create the Che workspace with the devfile as provided in this repository**

<img src="https://github.com/Rijul5/vscode-extension-che/raw/master/figs/che_workspace1.png" width="700" height="400" alt="Che Workspace">
=======

Retrieve the devfile.yaml by either cloning the project or downloading it locally and start a workspace from it:

```
chectl workspace:start -f devfile.yaml
```

**F) Code build run and test the vscode extension in che**

<img src="https://github.com/Rijul5/vscode-extension-che/raw/master/figs/start_hosted.png" width="700" height="400" alt="Theia Hosted Mode Start">
=======
The devfile is providing few commands to build, run and test the vscode extension in Che.

- `build ... hello-world vscode` builds the extension and packages it as `helloworld-1.0.0.vsix`
- `run ... remote helloworld vscode ext` Start the remote vscode extension.
- `run ... HOSTED che-theia + detect remote helloworld vscode ext` would start che-theia and discover all remote plugins: your remote vscode ext sidecar service would be detected.


<img src="https://github.com/Rijul5/vscode-extension-che/raw/master/figs/path_hosted.png" width="700" height="400" alt="Theia Hosted Mode Path">
=======
This devfile could be reuse as a template for your vscode extension. To have this work on yours:
- Change the source to point to your vscode extension
- The vscode extension sidecar container is the container containing the needed system dependencies to run your vscode extension.  For instance, if you need java for your extension you use `` instead of ``



<img src="https://github.com/Rijul5/vscode-extension-che/raw/master/figs/extension_test.png" width="700" height="400" alt="Command Execution">

