# Sample Helm CD solution using Reliza Hub

This solution utilizes CronJob running Helm on Kubernetes which is polling Reliza Hub for latest images and latest approved version of helm chart to use.

## Install

1. Have your project publishing Helm chart registered on Reliza Hub and obtain its project id
2. Create your Instance on [Reliza Hub](https://relizahub.com) and obtain Instance API ID and API Key.
3. Issue following commands via kubectl replacing <RELIZA_PROJECT_ID>, <RELIZA_API_ID> and <RELIZA_API_KEY> with values obtained from Reliza Hub. Note that in the below example application will be deployed into *myapp-namespace* while Helm CD CronJob will be deployed into *helm-reliza-cd* namespace.

```
kubectl create ns myapp-namespace
kubectl create ns helm-reliza-cd
kubectl create secret generic helm-reliza-cd-credentials -n helm-reliza-cd --from-literal=reliza-api-id=<RELIZA_API_ID> --from-literal=reliza-api-key=<RELIZA_API_KEY> --from-literal=reliza-project-id=<RELIZA_PROJECT_ID>
kubectl create configmap helm-reliza-cd-configmap -n reliza-cd-credentials --from-literal=namespace=myapp-namespace --from-literal=helm-release-name=myapp --from-literal=base-values-file=values.yaml
kubectl apply -f https://raw.githubusercontent.com/relizaio/reliza-hub-integrations/master/Helm-cd-with-Reliza/helm_configmap.yaml -n helm-reliza-cd
kubectl apply -f https://raw.githubusercontent.com/relizaio/reliza-hub-integrations/master/Helm-cd-with-Reliza/helmcron.yaml -n helm-reliza-cd
```

## Notes

Essentially, CD script is located in *helm_configmap.yaml* - it can be further edited based on specific project requirements.