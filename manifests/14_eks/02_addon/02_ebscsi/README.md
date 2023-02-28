# EBS CSI Driver Controller

```bash
eksctl get iamserviceaccount --cluster myeks --name ebs-csi-controller-sa
```

```bash
eksctl create addon --name aws-ebs-csi-driver --cluster myeks --service-account-role-arn <ROLE_ARN> --force
```

```bash
eksctl get addon --cluster myeks
```

```bash
kubectl get pod -n kube-system -l "app.kubernetes.io/component=csi-driver"
```

