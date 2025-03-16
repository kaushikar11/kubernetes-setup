# Kubernetes Local Environment Setup

This guide will help you set up a local Kubernetes environment using Minikube and kubectl on Windows.

## Prerequisites

- Windows operating system
- Administrative privileges
- Internet connection
- VirtualBox (recommended) or another virtualization platform (Docker, Hyper-V)

## Installation Steps

### 1. Installing kubectl

1. Download kubectl v1.32.3 for Windows AMD64 from the official Kubernetes releases page:
   - Visit: https://kubernetes.io/releases/download/#binaries
   - Download: `kubectl.exe` (v1.32.3, Windows, AMD64)

2. Create a directory and move kubectl:
   - Create a folder: `C:\Program Files\kubectl`
   - Move the downloaded `kubectl.exe` to this folder

3. Add kubectl to your PATH:
   - Open the Start menu and search for "Environment Variables"
   - Click "Edit the system environment variables"
   - Click the "Environment Variables" button
   - Under "System variables", find and select "Path"
   - Click "Edit"
   - Click "New" and add `C:\Program Files\kubectl`
   - Click "OK" on all dialog boxes to save the changes

4. Verify the installation:
   - Close any open Command Prompt windows
   - Open a new Command Prompt
   - Run: `kubectl --client version`
   - You should see the client version information

### 2. Installing Minikube

1. Install Minikube using PowerShell:
   - Open PowerShell as Administrator
   - Run the following commands:

   ```powershell
   New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
   Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing
   ```

2. Add Minikube to your PATH:
   - Run the following command in PowerShell:

   ```powershell
   $oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
   if ($oldPath.Split(';') -inotcontains 'C:\minikube'){
     [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine)
   }
   ```

3. Verify Minikube installation:
   - Close PowerShell
   - Open a new PowerShell window
   - Run: `minikube version`
   - You should see the Minikube version information

### 3. Starting Minikube

1. Start Minikube:
   - Run: `minikube start`
   - This will download VM components and start a local Kubernetes cluster

2. Troubleshooting startup issues:
   - If you encounter issues, try:
     ```
     minikube delete
     minikube start
     ```

   - If VirtualBox host-only adapter issues persist, try specifying a different driver:
     ```
     minikube start --driver=hyperv
     ```
     or
     ```
     minikube start --driver=docker
     ```

3. Verify cluster:
   - Run: `kubectl get nodes`
   - You should see the Minikube node running

4. Check VirtualBox:
   - Open VirtualBox to verify that the Minikube VM is running

## Exploring Kubernetes

Now that you have a running Kubernetes cluster, you can practice with:

1. Basic Kubernetes commands:
   - Explore the [Minikube handbook](https://minikube.sigs.k8s.io/docs/handbook/)
   - Follow tutorials at [Learning Ocean](https://learning-ocean.com/tutorials/kubernetes/kubernetes-minikube/)

2. Useful commands to get started:
   - `kubectl get pods --all-namespaces` - List all pods
   - `minikube dashboard` - Open the Kubernetes dashboard
   - `kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10` - Create a deployment
   - `kubectl expose deployment hello-minikube --type=NodePort --port=8080` - Expose a service

## Additional Resources

- [Official Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Troubleshooting

If you encounter issues with VirtualBox:
- Ensure you have at least VirtualBox 5.0.12 or newer
- Try removing existing host-only network adapters in VirtualBox's Host Network Manager
- Consider using an alternative driver (Docker or Hyper-V)

For any other issues, consult the [Minikube GitHub issues](https://github.com/kubernetes/minikube/issues) or [Kubernetes forum](https://discuss.kubernetes.io/).