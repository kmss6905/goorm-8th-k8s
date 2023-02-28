# Manual Node Scaling

```bash
eksctl scale nodegroup --cluster myeks --name mynbodes-t3 -N 3
```

# Cluster Autoscaler

```bash
curl -O https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml
```

`cluster-autoscaler-autodiscover.yaml`

```yml
...
apiVersion: apps/v1
kind: Deployment
...
spec:
  template
    metadata:
      ...
      annotations:
        ...
        cluster-autoscaler.kubernetes.io/safe-to-evect: 'false'
    spec:
      serviceAccountName: cluster-autoscaler
      containers:
        - image: k8s.gcr.io/autoscaling/cluster-autoscaler:v1.24.0
            ...
          command:
            - ./cluster-autoscaler
            - --v=4
            - --stderrthreshold=info
            - --cloud-provider=aws
            - --skip-nodes-with-local-storage=false
            - --expander=least-waste
            - --node-group-auto-discovery=asg:tag=k8s.io/cluster-autoscaler/enabled,k8s.io/cluster-autoscaler/myeks
            - --balance-similar-node-groups
            - --skip-nodes-with-system-pods=false
...
```

```bash
kubectl create -f cluster-autoscaler-autodiscover.yaml
```

```bash
kubectl logs -f deployment/cluster-autoscaler -n kube-system
```
