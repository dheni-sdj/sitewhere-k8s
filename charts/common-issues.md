# Common Issues

## Troubleshooting Techniques

### Rook Instalation

Verify the `rook-ceph-agent` pods are `Running`

```console
kubectl -n rook-ceph-system get pod
```

Verify that there are not error on `rook-ceph-agent` pods by running:

```console
kubectl -n rook-ceph-system get pod -l app=rook-ceph-agent -o jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.status.containerStatuses[0].lastState.terminated.message}{"\n"}{end}'
```

### Running on MiniKube

If you are running on `minikube` you can skip using `Rook.io`
and use the `values-dev.yaml`.

```console
helm install --name sitewhere \
-f ./sitewhere/values-dev.yaml \
--set sitewhere-infra-database.mongodb.persistence.storageClass=standard \
--set sitewhere-infra-core.kafka.persistence.storageClass=standard \
--set sitewhere-infra-core.kafka.zookeeper.persistence.storageClass=standard \
--set sitewhere-infra-core.kafka.external.enabled=false \
--set services.initContainers=false \
./sitewhere
```

Alternative, you can use sitewhere Helm Repo with:

```console
helm install --name sitewhere \
--set sitewhere-infra-core.kafka.zookeeper.replicaCount=1 \
--set sitewhere-infra-core.consul.Replicas=1 \
--set sitewhere-infra-database.mongodb.replicaSet.enabled=false \
--set sitewhere-infra-database.mongodb.persistence.storageClass=standard \
--set sitewhere-infra-core.kafka.persistence.storageClass=standard \
--set sitewhere-infra-core.kafka.zookeeper.persistence.storageClass=standard \
--set sitewhere-infra-core.kafka.external.enabled=false \
--set services.initContainers=false \
sitewhere/sitewhere
```

### Running on GKE

```console
helm install --name sitewhere \
--set sitewhere-infra-database.mongodb.persistence.storageClass=standard \
--set sitewhere-infra-core.kafka.persistence.storageClass=standard \
--set sitewhere-infra-core.kafka.zookeeper.persistence.storageClass=standard \
--set sitewhere-infra-core.kafka.external.enabled=false \
--set services.initContainers=false \
sitewhere/sitewhere
```

### Running on Amazon EKS

```console
helm install --name sitewhere \
-f values-eks.yaml \
sitewhere/sitewhere
```

### Minimal evironment with `hostpath` storageClass

```console
helm install --name sitewhere \
--set services.profile=minimal \
--set sitewhere-infra-core.kafka.zookeeper.replicaCount=1 \
--set sitewhere-infra-core.consul.Replicas=1 \
--set sitewhere-infra-database.mongodb.persistence.storageClass=hostpath \
--set sitewhere-infra-core.kafka.persistence.storageClass=hostpath \
--set sitewhere-infra-core.kafka.zookeeper.persistence.storageClass=hostpath \
--set sitewhere-infra-core.kafka.external.enabled=false \
--set services.initContainers=false \
sitewhere/sitewhere
```

### Developer evironment with `hostpath` storageClass 

```console
helm install --name sitewhere \
-f ./sitewhere/values-dev.yaml \
--set sitewhere-infra-database.mongodb.persistence.storageClass=hostpath \
--set sitewhere-infra-core.kafka.persistence.storageClass=hostpath \
--set sitewhere-infra-core.kafka.zookeeper.persistence.storageClass=hostpath \
./sitewhere
```