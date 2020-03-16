# littleci

This is a simple example to deploy a CI/CD environment on a kubernetes cluster

```bash
# Deploy gitea
kubectl create -f gitea.yaml

# Deploy drone
helm install drone https://github.com/gtaylor/drone-charts/releases/download/drone-0.1.3/drone-0.1.3.tgz --values drone/drone-values.yaml
helm install drone-runner https://github.com/gtaylor/drone-charts/releases/download/drone-runner-kube-0.1.2/drone-runner-kube-0.1.2.tgz --values drone/drone-runner-values.yaml
```
