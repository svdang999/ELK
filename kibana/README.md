# Steps to install Kibana on Kubernetes using Helm chart version 3
helm repo add elastic https://helm.elastic.co
helm repo update
helm install kibana elastic/kibana -f custom-values.yaml
