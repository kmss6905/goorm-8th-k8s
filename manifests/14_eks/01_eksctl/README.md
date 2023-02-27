# Eksctl

클러스터 생성

```bash
eksctl create cluster -f myeks.yaml
```

클러스터 확인

```bash
eksctl get cluster
```

노드 그룹 확인

```bash
eksctl get nodegroup --cluster myeks
```

Fargate 프로파일 확인

```bash
eksctl get fargateprofile --cluster myeks
```

IAM 서비스 계정 확인

```bash
eksctl get iamserviceaccount --cluster myeks
```

클러스터 삭제

```bash
eksctl delete cluster -f myeks.yaml
```
