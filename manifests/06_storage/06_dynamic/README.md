# Dynamic Provisioning

```bash
cd ~
git clone https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner
```

```bash
cd ~/nfs-subdir-external-provisioner/deploy/
```

`deployments.yaml`
```bash
...
      containers:
        ...
          env:
            - name: PROVISIONER_NAME
              value: k8s-sigs.io/nfs-subdir-external-provisioner
            - name: NFS_SERVER
              value: 192.168.56.11
            - name: NFS_PATH
              value: /srv/nfs-volume
      volumes:
        - name: nfs-client-root
          nfs:
            server: 192.168.56.11
            path: /srv/nfs-volume
```

```bash
kubectl apply -k .
```

```bash
kubectl patch storageclasses.storage.k8s.io nfs-client -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```
