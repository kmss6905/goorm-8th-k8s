# IAM User for EKS Cluster

IAM 사용자 생성: eksuser1

`~/.aws/credentials`

```bash
[default]
aws_access_key_id = AKIXXX
aws_secret_access_key = aGbm8XXX

[eksuser1]
aws_access_key_id = AKIXXX
aws_secret_access_key = mVoLGXXX
```

```bash
aws configure list-profiles
```

```bash
export AWS_PROFILE=eksuser1
aws sts get-caller-identity --no-cli-pager
```

```bash
export AWS_PROFILE=default
aws sts get-caller-identity --no-cli-pager
```

---

```bash
kubectl get cm -n kube-system aws-auth -o yaml > aws-auth.yaml
```

`aws-auth.yaml`

```yaml
apiVersion: v1
data:
  ...
    mapUsers: |
    - userarn: <ARN>
      username: eksuser1
      groups:
      - system:masters
...
```

```bash
kubectl apply -f aws-auth.yaml
```

```bash
export AWS_PROFILE=eksuser1
kubectl get nodes
kubectl create deploy myweb --image nginx
kubectl get deploy
kubectl get po
kubectl get clusterroles
kubectl delete deploy myweb
```

```bash
export AWS_PROFILE=default
```

