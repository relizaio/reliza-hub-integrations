# Starting Reliza Client Watcher on Kubernetes

1. Create your Instance on [Reliza Hub](https://relizahub.com) and obtain Instance API ID and API Key.
2. Issue following commands via kubectl replacing <RELIZA_API_ID> and <RELIZA_API_KEY> with values obtained from Reliza Hub:

```
kubectl create ns reliza-client
kubectl create secret generic reliza-client-api-key -n reliza-client --from-literal=reliza-api-id=<RELIZA_API_ID> --from-literal=reliza-api-key=<RELIZA_API_ID>
kubectl create configmap reliza-client-configmap -n reliza-client --from-literal=namespace=allnamespaces
kubectl apply -f https://raw.githubusercontent.com/relizaio/reliza-hub-integrations/master/kubernetes/reliza-client-watcher.yaml -n reliza-client
```

**Note:** If you would like to watch only specific namespaces, say *default* and *myappnamespace*, use the following syntax instead when creating configmap (comma-separated namespaces as a value for namespace key):

```
kubectl create configmap reliza-client-configmap -n reliza-client --from-literal=namespace=default,myappnamespace
```