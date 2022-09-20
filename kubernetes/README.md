# Reliza Watcher for Kubernetes

## Install Using YAML

1. Create your Instance on [Reliza Hub](https://relizahub.com) and obtain Instance API ID and API Key.
2. Issue following commands via kubectl replacing <RELIZA_API_ID> and <RELIZA_API_KEY> with values obtained from Reliza Hub:

```
kubectl create ns reliza-watcher
kubectl create secret generic reliza-watcher-api-key -n reliza-watcher --from-literal=reliza-api-id=<RELIZA_API_ID> --from-literal=reliza-api-key=<RELIZA_API_KEY>
kubectl create configmap reliza-watcher-configmap -n reliza-watcher --from-literal=sender=default
kubectl apply -f https://raw.githubusercontent.com/relizaio/reliza-hub-integrations/master/kubernetes/reliza-watcher.yaml -n reliza-watcher
```

**Notes:**

1. This will watch all the namespaces in the cluster; if you would like to watch only specific namespaces, install using helm.

2. Sender id can be set to any string via *sender* key in the config map. Data from different senders will be combined on the Reliza Hub in the instance view.


## Install Using Helm (Recommended)

Refer to instructions in the Reliza Watcher Helm chart here - https://github.com/relizaio/helm-charts#2-reliza-watcher-helm-chart
