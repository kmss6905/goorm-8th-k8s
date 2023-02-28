# AWS LB Controller

```bash
helm repo add eks https://aws.github.io/eks-charts
```

```bash
helm repo update
```

```bash
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=myeks \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller
```

```bash
kubectl get deployment -n kube-system aws-load-balancer-controller
```

