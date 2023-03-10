# Network Policy Example

```shell
kubectl create deploy myweb --image nginx
kubectl expose deploy myweb --name myweb-svc --port 80 --target-port 80
```

```shell
kubectl get po --show-labels
```

`netpol.yaml`
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-wc1
spec:
  podSelector:
    matchLabels:
      app: myweb
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: wc1
      ports:
        - protocol: TCP
          port: 80
```

```shell
kubectl create -f netpol.yaml
```

```shell
kubectl run wc1 --image ghcr.io/c1t1d0s7/network-multitool -l app=wc1 --rm -it -- sh

> curl myweb-svc
```

```shell
kubectl run wc2 --image ghcr.io/c1t1d0s7/network-multitool -l app=wc2 --rm -it -- sh

> curl myweb-svc
```


