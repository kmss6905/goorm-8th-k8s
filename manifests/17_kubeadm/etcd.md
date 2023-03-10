# Etcd 백업/복구

```shell
sudo apt install etcd-client
```

```shell
sudo -i
```

```shell
alias etcdctl='ETCDCTL_API=3 etcdctl --endpoints 127.0.0.1:2379 \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt'
```

ETCD 클러스터 맴버 목록
```shell
etcdctl member list
```

백업
```shell
etcdctl snapshot save snapshotdb
```

복구
```shell
etcdctl snapshot restore snapshotdb
```

