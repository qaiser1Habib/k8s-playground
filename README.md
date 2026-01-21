# Kubernetes Cluster Setup

This repository provides configuration and instructions for setting up a local Kubernetes cluster using KIND (Kubernetes IN Docker) on macOS. It also includes guidance for working with cloud-managed clusters like EKS or GKE.

---

## Prerequisites

- **Docker Desktop** (running)
- **kubectl** installed
- **Homebrew** (optional, for installing KIND)

Verify installation:

```bash
docker --version
kubectl version --client
kind version
```

---

## Installing KIND

Install KIND via Homebrew:

```bash
brew install kind
```

---

## Creating a Local Cluster

1. **Using default configuration:**

```bash
kind create cluster
kubectl get nodes
```

2. **Using custom configuration (`config.yml`):**

```bash
kind create cluster --name k8s-cluster --config=config.yml
```

Example `config.yml`:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
  - role: control-plane
    image: kindest/node:v1.31.2
    extraPortMappings:
      - containerPort: 80
        hostPort: 80
        protocol: TCP
      - containerPort: 443
        hostPort: 443
        protocol: TCP

  - role: worker
    image: kindest/node:v1.31.2

  - role: worker
    image: kindest/node:v1.31.2
```

---

## Using the Cluster

- Check nodes:

```bash
kubectl get nodes
```

- Apply manifests:

```bash
kubectl apply -f deployment.yml
```

- Delete cluster when done:

```bash
kind delete cluster --name k8s-cluster
```

---

## Notes for macOS

- Do **not** use `systemctl` commands; Docker runs as a desktop app.  
- Make sure Docker Desktop is running before creating the cluster.  

---

## Cloud Alternatives

For production-grade environments, managed Kubernetes services are commonly used:

- **AWS EKS** – Fully managed on AWS  
- **GKE** – Managed Kubernetes on Google Cloud  
- **AKS** – Managed Kubernetes on Azure  

---

## References

- [KIND Documentation](https://kind.sigs.k8s.io/)  
- [Kubernetes Official Docs](https://kubernetes.io/docs/home/)  
- [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop/)

---

## License

This project is licensed under the MIT License.