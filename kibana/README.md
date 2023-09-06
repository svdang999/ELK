# Source repo
```
https://artifacthub.io/packages/helm/elastic/kibana
```

# Steps to install Kibana on Kubernetes using Helm chart version 3
```
helm repo add elastic https://helm.elastic.co
helm repo update
helm install kibana elastic/kibana -f custom-values.yaml
```

# Steps to install Kibana on Kubernetes using deployment file
```
kubectl -n logging apply -f kibana-deployment.yaml
```
