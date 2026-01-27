# Kubernetes File Writer Demo

This project demonstrates a simple Kubernetes application consisting of a Python file writer, a deployment configuration, and auto-scaling capabilities.

## Description

The core application is a Python script that writes timestamps to a file. It is designed to demonstrate Kubernetes concepts such as `hostPath` volumes, resource limits, and Horizontal Pod Autoscaling (HPA). The writer can also run in a "stress" mode to consume CPU, simulating load to trigger the HPA.

## Components

The project consists of the following Kubernetes resources:

### `pod.yaml`
Defines a standalone Pod named `file-writer-pod`.
- **Image**: `python:3.11-slim`
- **Command**: Runs `writer.py` to write to `/app/output.log`.
- **Volume**: Mounts `/mnt/myapp` from the host to `/app` in the container.

### `writer.yaml`
Defines a Deployment named `file-writer` with 1 replica.
- **Image**: `python:3.11-slim`
- **Command**: Runs `writer.py` with the `--stress` flag to generate CPU load.
- **Resources**:
  - Requests: 100m CPU, 128Mi Memory
  - Limits: 200m CPU, 250Mi Memory
- **Volume**: Mounts `/mnt/myapp` from the host to `/app`.

### `writer-scaling.yaml`
Defines a Horizontal Pod Autoscaler (HPA) named `file-writer-hpa`.
- **Target**: `file-writer` Deployment.
- **Scaling**: Scales between 1 and 5 replicas.
- **Metric**: Target CPU utilization is 50%.

## Usage

### Prerequisites
- A Kubernetes cluster (e.g., Minikube, Docker Desktop).
- `kubectl` installed and configured.
- **IMPORTANT**: Ensure the directory `/mnt/myapp` exists on the Kubernetes node (or your local machine if using Docker Desktop/Minikube with host path mounting enabled).

### Deployment

1. **Apply the resources:**

   ```bash
   kubectl apply -f writer.yaml
   kubectl apply -f writer-scaling.yaml
   ```

   *Alternatively, to run just the standalone pod:*
   ```bash
   kubectl apply -f pod.yaml
   ```

2. **Verify the deployment:**

   ```bash
   kubectl get deployments
   kubectl get hpa
   ```

3. **Check the logs:**

   ```bash
   kubectl logs -f deployment/file-writer
   ```

4. **Observe Scaling:**
   Since `writer.yaml` runs with `--stress`, the CPU usage should increase, triggering the HPA to scale up the replicas. Watch it happen with:

   ```bash
   kubectl get hpa -w
   ```
