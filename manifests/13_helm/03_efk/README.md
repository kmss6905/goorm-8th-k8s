# EFK

```bash
kubectl create ns logging
```

```bash
helm repo add elastic https://helm.elastic.co
helm repo update
```

```bash
helm show values --version 7.17.3 elastic/elasticsearch > elasticsearch-value.yaml
```

`elasticsearch-value.yaml`
```yaml
 18 replicas: 1
 19 minimumMasterNodes: 1
...
 80 resources:
 81   requests:
 82     cpu: "1000m"
 83     memory: "1Gi"
 84   limits:
 85     cpu: "1000m"
 86     memory: "1Gi"
```

```bash
helm install elasticsearch elastic/elasticsearch --version 7.17.3 -f elasticsearch-value.yaml -n logging
```

---

```bash
helm repo add fluent https://fluent.github.io/helm-charts
helm repo update
```

```bash
helm install fluent-bit fluent/fluent-bit -n logging
```

---

```bash
helm show values --version 7.17.3 elastic/kibana > kibana-value.yaml
```

`kibana-value.yaml`
```yaml
 49 resources:
 50   requests:
 51     cpu: "1000m"
 52     memory: "1Gi"
 53   limits:
 54     cpu: "1000m"
 55     memory: "1Gi"
...
119 service:
120   type: LoadBalancer
```

```bash
helm install kibana elastic/kibana --version 7.17.3 -f kibana-value.yaml -n logging
```

> http://192.168.56.X:5601

1. Welcome 페이지: `Explorer on my own` 버튼
2. 좌측 상단 햄버거 매뉴: Management -> Stack Management 선택
3. Kibana -> Index Patterns 선택
4. Create Index pattern 선택
5. Name: `logstash-*` 입력
6. Temestamp filed: `@timestamp` 선택
7. `Create index pattern` 버튼
8. 좌측 햄버거 메뉴 -> Analytics -> Discover
9. 로그 검색

