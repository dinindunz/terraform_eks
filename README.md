# Provision an EKS Cluster with Terraform

Configure kubectl
aws eks --region $(terraform output -raw region) update-kubeconfig --name $(terraform output -raw cluster_name)

Deploy Kubernetes Metrics Server
```
wget -O v0.3.6.tar.gz https://codeload.github.com/kubernetes-sigs/metrics-server/tar.gz/v0.3.6 && tar -xzf v0.3.6.tar.gz
kubectl apply -f metrics-server-0.3.6/deploy/1.8+/
kubectl get deployment metrics-server -n kube-system
```

Deploy Kubernetes Dashboard
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta8/aio/deploy/recommended.yaml
kubectl proxy
```

Authenticate the dashboard
```
kubectl apply -f https://raw.githubusercontent.com/hashicorp/learn-terraform-provision-eks-cluster/main/kubernetes-dashboard-admin.rbac.yaml
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep service-controller-token | awk '{print $1}')
```