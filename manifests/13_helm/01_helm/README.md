# Helm

```bash
cd ~
wget https://get.helm.sh/helm-v3.11.1-linux-amd64.tar.gz
```

```bash
tar xf helm-v3.11.1-linux-amd64.tar.gz
sudo install linux-amd64/helm /usr/local/bin
```

```bash
helm version
```

```bash
helm repo list
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo list
```

```bash
helm repo update
```

```bash
helm search repo bitnami
helm search repo bitnami/mysql
```

```bash
helm list
helm install mydb bitnami/mysql
helm list
helm status mydb
```

```bash
kubectl get all
kubectl get pvc,pv
```

```bash
helm uninstall mydb
kubectl get all
kubectl get pvc,pv
kubectl delete pvc --all
```

