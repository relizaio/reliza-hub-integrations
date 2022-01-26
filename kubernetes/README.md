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


## Install Using Helm

Create your Instance on [Reliza Hub](https://relizahub.com) and obtain Instance API ID and API Key.

```
kubectl create ns reliza-watcher
kubectl create secret generic reliza-watcher-api-key -n reliza-watcher --from-literal=reliza-api-id=<RELIZA_API_ID> --from-literal=reliza-api-key=<RELIZA_API_KEY>
helm install reliza-watcher -n reliza-watcher ./kubernetes/reliza-watcher-helm
```

**Notes:**

1. If you would like to watch only specific namespaces, say *default* and *myappnamespace*, use the following syntax instead when installing helmchart (comma-separated namespaces as a value for namespace key).

```
helm install reliza-watcher -n reliza-watcher ./kubernetes/reliza-watcher-helm --set namespace="default\,myappnamespace"
```

2. Sender id can be set to any string via *sender* key using --set flag. Data from different senders will be combined on the Reliza Hub in the instance view.

## Install Using Helm In A Multi-Namespace Kubernetes Clusters

When you wish to watch different instances deployed in different namespaces of a kubernetes cluster, multiple instances of reliza-watcher are required and can be deployed as follows:

Assume you have two instances *instance-A* and *instance-B* on [Reliza Hub](https://relizahub.com) deployed in namespaces *ns-A*  and *ns-B* respectively. Then:

1. Obtain Instance API ID and API Key for the instances *instance-A* and *instance-B* from [Reliza Hub](https://relizahub.com).
2. Issue following commands replacing <RELIZA_API_ID_FOR_INSTANCE_A> and <RELIZA_API_KEY_INSTANCE_A> with values obtained from Reliza Hub:
```
kubectl create secret generic reliza-watcher-api-key -n <ns-A> --from-literal=reliza-api-id=<RELIZA_API_ID_FOR_INSTANCE_A> --from-literal=reliza-api-key=<RELIZA_API_KEY_INSTANCE_A>
helm install reliza-watcher -n <ns-A> ./kubernetes/reliza-watcher-helm --set namespace="ns-A"
```
3. Issue following commands replacing <RELIZA_API_ID_FOR_INSTANCE_B> and <RELIZA_API_KEY_INSTANCE_B> with values obtained from Reliza Hub:
```
kubectl create secret generic reliza-watcher-api-key -n <ns-B> --from-literal=reliza-api-id=<RELIZA_API_ID_FOR_INSTANCE_B> --from-literal=reliza-api-key=<RELIZA_API_KEY_INSTANCE_B>
helm install reliza-watcher -n <ns-B> ./kubernetes/reliza-watcher-helm --set namespace="ns-B"
```
