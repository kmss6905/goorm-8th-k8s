# Jenkins with AWS EKS

## Deploy EKS

```bash
eksctl create cluster --name myeks --region ap-northeast-2 --version 1.24 --instance-types t3.medium
```

```bash
eksctl utils associate-iam-oidc-provider --cluster myeks --approve
```

```bash
eksctl create iamserviceaccount \
  --name ebs-csi-controller-sa \
  --namespace kube-system \
  --cluster myeks \
  --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy \
  --approve \
  --role-only \
  --role-name AmazonEKS_EBS_CSI_DriverRole
```

```bash
aws sts get-caller-identity --output text
```

```bash
eksctl create addon --name aws-ebs-csi-driver --cluster myeks --service-account-role-arn arn:aws:iam::<ACCOUNT_ID>:role/AmazonEKS_EBS_CSI_DriverRole --force
```

## Deploy Jenkins on EKS with Helm

```bash
kubectl create namespace jenkins
```

```bash
helm repo add jenkinsci https://charts.jenkins.io
```

```bash
helm repo update
```

`jenkins-values.yaml`

```yml
controller:
  tag: "lts-jdk11"
  serviceType: LoadBalancer
  installPlugins:
  - kubernetes
  - workflow-aggregator
  - git
  - configuration-as-code
  - pipeline-stage-view
  adminPassword: "P@ssw0rd"

persistence:
  storageClass: "gp2"
```

```bash
helm install jenkins jenkinsci/jenkins -n jenkins -f jenkins-values.yaml
```


