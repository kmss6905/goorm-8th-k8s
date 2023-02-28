# CloudWatch Container Insights

```bash
ClusterName=myeks
RegionName=ap-northeast-2
FluentBitHttpPort='2020'
FluentBitReadFromHead='Off'
[[ ${FluentBitReadFromHead} = 'On' ]] && FluentBitReadFromTail='Off'|| FluentBitReadFromTail='On'
[[ -z ${FluentBitHttpPort} ]] && FluentBitHttpServer='Off' || FluentBitHttpServer='On'
curl https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/latest/k8s-deployment-manifest-templates/deployment-mode/daemonset/container-insights-monitoring/quickstart/cwagent-fluent-bit-quickstart.yaml | sed 's/{{cluster_name}}/'${ClusterName}'/;s/{{region_name}}/'${RegionName}'/;s/{{http_server_toggle}}/"'${FluentBitHttpServer}'"/;s/{{http_server_port}}/"'${FluentBitHttpPort}'"/;s/{{read_from_head}}/"'${FluentBitReadFromHead}'"/;s/{{read_from_tail}}/"'${FluentBitReadFromTail}'"/' | kubectl apply -f -
```

```bash
kubectl get po -n amazon-cloudwatch
```

## 로그
`CloudWatch -> 로그 -> 로그 그룹`

- Control Plane
  - /aws/eks/myeks/cluster
- Node
  - /aws/containerinsights/myeks/application
  - /aws/containerinsights/myeks/host
  - /aws/containerinsights/myeks/performance
  - /aws/containerinsights/myeks/dataplane

## Performance Monitoring
`CloudWatch -> Container Insights`
