# Kubectl Basic Cheatsheet - Part Uno

Kubectl is like the swiss army knife when dealing with kubernetes cluster.

These are some basic hacks, which will be useful to someone.

## Dealing with jsonpath

JSONPath template is composed of JSONPath expressions enclosed by {}. We can use JSONPath expressions to filter on specific fields from kubectl outputs.

### Good Reads

- https://downey.io/notes/dev/kubectl-jsonpath-new-lines/
- https://gist.github.com/so0k/42313dbb3b547a0f51a547bb968696ba
- https://kubernetes.io/docs/reference/kubectl/jsonpath/
- https://unofficial-kubernetes.readthedocs.io/en/latest/user-guide/jsonpath/

### Printing the names of kubernetes deployments

```
kubectl get -n kube-system deploy -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'
```

See the sample output

```
$ kubectl get -n newrelic deploy -o jsonpath='{range .items[*]}{.metadata.name}{"\n"}{end}'
kelvin
nri-bundle-kube-state-metrics
nri-bundle-nri-kube-events
nri-bundle-nri-metadata-injection
nri-bundle-nrk8s-ksm
vizier-cloud-connector
vizier-query-broker
```

### Printing the IP Addresses of the all pods in newrelic namespace

```
kubectl get pods -n newrelic -o jsonpath='{range .items[*]}{.status.podIP}{"\n"}{end}'
```

See the sample output

```
$ kubectl get pods -n newrelic -o jsonpath='{range .items[*]}{.status.podIP}{"\n"}{end}'
172.16.0.179
172.16.1.17
172.16.1.198
172.16.1.61
172.16.0.183
172.16.0.187
172.16.1.132
172.16.1.178
172.16.1.11
172.16.0.178
172.16.1.167
172.16.1.214
172.16.1.161
172.16.0.81
172.16.1.209
172.16.1.61
172.16.0.183
172.16.1.198
172.16.1.102
```

You can use two fields and format it to your liking using whitespace as well.

```
kubectl get pods -n newrelic -o jsonpath='{range .items[*]}{.metadata.namespace}{"/"}{.metadata.name}{","}{.status.podIP}{"\n"}{end}'
```

See the sample output

```
$ kubectl get pods -n newrelic -o jsonpath='{range .items[*]}{.metadata.namespace}{"/"}{.metadata.name}{","}{.status.podIP}{"\n"}{end}'
newrelic/kelvin-54b954889c-4zsz5,172.16.0.179
newrelic/nri-bundle-kube-state-metrics-778f8d7756-6tgzg,172.16.1.17
newrelic/nri-bundle-newrelic-logging-9bctl,172.16.1.198
newrelic/nri-bundle-newrelic-logging-splhw,172.16.1.61
newrelic/nri-bundle-newrelic-logging-tfl7r,172.16.0.183
newrelic/nri-bundle-newrelic-prometheus-agent-0,172.16.0.187
newrelic/nri-bundle-nri-kube-events-695cc855d4-jtdtk,172.16.1.132
newrelic/nri-bundle-nri-metadata-injection-7bfddbc55d-khxrt,172.16.1.178
newrelic/nri-bundle-nrk8s-ksm-9bf8fbf54-fpp29,172.16.1.11
newrelic/nri-bundle-nrk8s-kubelet-22plc,172.16.0.178
newrelic/nri-bundle-nrk8s-kubelet-qzhhs,172.16.1.167
newrelic/nri-bundle-nrk8s-kubelet-vpmbq,172.16.1.214
newrelic/pl-nats-0,172.16.1.161
newrelic/vizier-cloud-connector-5d454b78f6-wq2v8,172.16.0.81
newrelic/vizier-metadata-0,172.16.1.209
newrelic/vizier-pem-6ctzn,172.16.1.61
newrelic/vizier-pem-vgbq5,172.16.0.183
newrelic/vizier-pem-wfbwk,172.16.1.198
newrelic/vizier-query-broker-7f57fbfc6d-5qckt,172.16.1.102
```
