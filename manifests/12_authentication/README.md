# SA - RBAC

```bash
kubectl create -f 02_service_account/
kubectl create -R -f 03_rbac/
```

`kubectl.yaml`
```yaml
...
spec:
  serviceAccountName: XXX
...
```

```bash
kubectl create -f kubectl.yaml
```

```bash
kubectl exec -it kubectl -- sh
```

