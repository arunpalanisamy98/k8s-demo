# Kubernetes File Writer Demo

A demo project showcasing a Python file writer app on Kubernetes with auto-scaling capabilities.

## Prerequisites
- **Docker**: Container runtime.
- **Minikube**: Local Kubernetes cluster.

## Installation

### 1. Install kubectl
For macOS (via Homebrew):
```bash
brew install kubectl
```
For Linux (via Snap):
```bash
sudo snap install kubectl --classic
```

### 2. Start Minikube
Initialize Minikube with the Docker driver and enable necessary addons:
```bash
minikube start --driver=docker
minikube status
minikube addons enable metrics-server
```

## Setup

### Mount Application Volume
Mount your local application directory to the Minikube node.
*Replace `/path/to/react_app` with your actual local path.*
```bash
minikube mount /path/to/react_app:/mnt/myapp
```

## Deployment

### 1. Apply Configurations
Start the specific components (ensure you use the correct paths to your YAML files):

**Start the Horizontal Pod Autoscaler (HPA):**
```bash
kubectl apply -f /path/to/writer-scaling.yaml
```

**Start the Deployment:**
```bash
kubectl apply -f /path/to/writer.yaml
```

### 2. Verify Scaling
Observe the pods scaling up as the application generates load:
```bash
kubectl get hpa -w
```
