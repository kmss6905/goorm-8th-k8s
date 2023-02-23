# x.509

```bash
openssl x509 -in /etc/kubernetes/pki/ca.crt -text -noout
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```

```bash
openssl genrsa -out myuser3.key 2048
```

```bash
openssl req -new -key myuser3.key -out myuser3.csr -subj "/CN=myuser3"
```

```bash
base64 myuser3.csr -w 0
```

`myuser3-csr.yaml`
```yaml
...
spec:
  request: <PASTE>
...
```

```bash
kubectl create -f myuser3-csr.yaml
```

```bash
kubectl get csr
```

```bash
kubectl certifiacte approve myuser3
```

```bash
kubectl get csr
```

```bash
kubectl get csr myuser3 -o jsonpath='{.status.certificate}' | base64 -d > myuser3.crt
```

```bash
openssl x509 -in myuser3.crt -text -noout
```

