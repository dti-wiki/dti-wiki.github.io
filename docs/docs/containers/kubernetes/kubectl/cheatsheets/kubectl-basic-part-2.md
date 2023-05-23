# Kubectl Basic Cheatsheet - Part Duo

Continuation of https://dti-wiki.github.io/containers/kubernetes/kubectl/cheatsheets/kubectl-basic-part-1/

## Format Kubectl output as rows and columns

### Formatting Output as Rows

```
kubectl get pods -n newrelic -o jsonpath='{range .items[*]}{"pod: "}{.metadata.name}{"\n"}{range .spec.containers[*]}{"\tname: "}{.name}{"\n\timage: "}{.image}{"\n"}{end}'
```

This should give output as below:

```
pod: kelvin-54b954889c-4zsz5
        name: app
        image: gcr.io/pixie-oss/pixie-prod/vizier/kelvin_image:0.13.4
pod: nri-bundle-kube-state-metrics-778f8d7756-6tgzg
        name: kube-state-metrics
        image: registry.k8s.io/kube-state-metrics/kube-state-metrics:v2.6.0
pod: nri-bundle-newrelic-logging-9bctl
        name: newrelic-logging
        image: newrelic/newrelic-fluentbit-output:1.14.0
pod: nri-bundle-newrelic-logging-splhw
        name: newrelic-logging
        image: newrelic/newrelic-fluentbit-output:1.14.0
pod: nri-bundle-newrelic-logging-tfl7r
        name: newrelic-logging
        image: newrelic/newrelic-fluentbit-output:1.14.0
pod: nri-bundle-newrelic-prometheus-agent-0
        name: prometheus
        image: quay.io/prometheus/prometheus:v2.37.3
pod: nri-bundle-nri-kube-events-695cc855d4-jtdtk
        name: kube-events
        image: newrelic/nri-kube-events:1.9.1
        name: forwarder
        image: newrelic/k8s-events-forwarder:1.33.1
pod: nri-bundle-nri-metadata-injection-7bfddbc55d-khxrt
        name: nri-metadata-injection
        image: newrelic/k8s-metadata-injection:1.7.5
pod: nri-bundle-nrk8s-ksm-9bf8fbf54-fpp29
        name: ksm
        image: newrelic/nri-kubernetes:3.6.0
        name: forwarder
        image: newrelic/k8s-events-forwarder:1.33.2
pod: nri-bundle-nrk8s-kubelet-22plc
        name: kubelet
        image: newrelic/nri-kubernetes:3.6.0
        name: agent
        image: newrelic/infrastructure-bundle:2.8.34
pod: nri-bundle-nrk8s-kubelet-qzhhs
        name: kubelet
        image: newrelic/nri-kubernetes:3.6.0
        name: agent
        image: newrelic/infrastructure-bundle:2.8.34
pod: nri-bundle-nrk8s-kubelet-vpmbq
        name: kubelet
        image: newrelic/nri-kubernetes:3.6.0
        name: agent
        image: newrelic/infrastructure-bundle:2.8.34
pod: pl-nats-0
        name: pl-nats
        image: gcr.io/pixie-oss/pixie-prod/vizier-deps/nats:multiarch-2.8.4-alpine3.15@sha256:988010f74b61749cfb82c28b50b47d26d0b972860ce6e9325a5afcde97713da2
pod: vizier-cloud-connector-5d454b78f6-wq2v8
        name: app
        image: gcr.io/pixie-oss/pixie-prod/vizier/cloud_connector_server_image:0.13.4
pod: vizier-metadata-0
        name: app
        image: gcr.io/pixie-oss/pixie-prod/vizier/metadata_server_image:0.13.4
pod: vizier-pem-6ctzn
        name: pem
        image: gcr.io/pixie-oss/pixie-prod/vizier/pem_image:0.13.4
pod: vizier-pem-vgbq5
        name: pem
        image: gcr.io/pixie-oss/pixie-prod/vizier/pem_image:0.13.4
pod: vizier-pem-wfbwk
        name: pem
        image: gcr.io/pixie-oss/pixie-prod/vizier/pem_image:0.13.4
pod: vizier-query-broker-7f57fbfc6d-5qckt
        name: app
        image: gcr.io/pixie-oss/pixie-prod/vizier/query_broker_server_image:0.13.4
```

### Formatting Output as Columns

```
kubectl get pod -n newrelic -o="custom-columns=PODS:.metadata.name,NAME:.spec.containers[*].name,IMAGE:.spec.containers[*].image,INIT-CONTAINERS:.spec.initContainers[*].name,CONTAINERS:.spec.containers[*].name"
```

This should give output as below:

```
PODS                                                 NAME                     IMAGE                                                                                                                                             INIT-CONTAINERS    CONTAINERS
kelvin-54b954889c-4zsz5                              app                      gcr.io/pixie-oss/pixie-prod/vizier/kelvin_image:0.13.4                                                                                            cc-wait,qb-wait    app
nri-bundle-kube-state-metrics-778f8d7756-6tgzg       kube-state-metrics       registry.k8s.io/kube-state-metrics/kube-state-metrics:v2.6.0                                                                                      <none>             kube-state-metrics
nri-bundle-newrelic-logging-9bctl                    newrelic-logging         newrelic/newrelic-fluentbit-output:1.14.0                                                                                                         <none>             newrelic-logging
nri-bundle-newrelic-logging-splhw                    newrelic-logging         newrelic/newrelic-fluentbit-output:1.14.0                                                                                                         <none>             newrelic-logging
nri-bundle-newrelic-logging-tfl7r                    newrelic-logging         newrelic/newrelic-fluentbit-output:1.14.0                                                                                                         <none>             newrelic-logging
nri-bundle-newrelic-prometheus-agent-0               prometheus               quay.io/prometheus/prometheus:v2.37.3                                                                                                             configurator       prometheus
nri-bundle-nri-kube-events-695cc855d4-jtdtk          kube-events,forwarder    newrelic/nri-kube-events:1.9.1,newrelic/k8s-events-forwarder:1.33.1                                                                               <none>             kube-events,forwarder
nri-bundle-nri-metadata-injection-7bfddbc55d-khxrt   nri-metadata-injection   newrelic/k8s-metadata-injection:1.7.5                                                                                                             <none>             nri-metadata-injection
nri-bundle-nrk8s-ksm-9bf8fbf54-fpp29                 ksm,forwarder            newrelic/nri-kubernetes:3.6.0,newrelic/k8s-events-forwarder:1.33.2                                                                                <none>             ksm,forwarder
nri-bundle-nrk8s-kubelet-22plc                       kubelet,agent            newrelic/nri-kubernetes:3.6.0,newrelic/infrastructure-bundle:2.8.34                                                                               <none>             kubelet,agent
nri-bundle-nrk8s-kubelet-qzhhs                       kubelet,agent            newrelic/nri-kubernetes:3.6.0,newrelic/infrastructure-bundle:2.8.34                                                                               <none>             kubelet,agent
nri-bundle-nrk8s-kubelet-vpmbq                       kubelet,agent            newrelic/nri-kubernetes:3.6.0,newrelic/infrastructure-bundle:2.8.34                                                                               <none>             kubelet,agent
pl-nats-0                                            pl-nats                  gcr.io/pixie-oss/pixie-prod/vizier-deps/nats:multiarch-2.8.4-alpine3.15@sha256:988010f74b61749cfb82c28b50b47d26d0b972860ce6e9325a5afcde97713da2   <none>             pl-nats
vizier-cloud-connector-5d454b78f6-wq2v8              app                      gcr.io/pixie-oss/pixie-prod/vizier/cloud_connector_server_image:0.13.4                                                                            nats-wait          app
vizier-metadata-0                                    app                      gcr.io/pixie-oss/pixie-prod/vizier/metadata_server_image:0.13.4                                                                                   nats-wait          app
vizier-pem-6ctzn                                     pem                      gcr.io/pixie-oss/pixie-prod/vizier/pem_image:0.13.4                                                                                               qb-wait            pem
vizier-pem-vgbq5                                     pem                      gcr.io/pixie-oss/pixie-prod/vizier/pem_image:0.13.4                                                                                               qb-wait            pem
vizier-pem-wfbwk                                     pem                      gcr.io/pixie-oss/pixie-prod/vizier/pem_image:0.13.4                                                                                               qb-wait            pem
vizier-query-broker-7f57fbfc6d-5qckt                 app                      gcr.io/pixie-oss/pixie-prod/vizier/query_broker_server_image:0.13.4                                                                               cc-wait,mds-wait   app
```

### Formatting Output as Lines

```
kubectl get pods/nri-bundle-nri-kube-events-695cc855d4-jtdtk -n newrelic -o jsonpath='{range .spec.containers[*]}{.name}{"\n"}{end}'
```

This should give output as below:

```
kube-events
forwarder
```
