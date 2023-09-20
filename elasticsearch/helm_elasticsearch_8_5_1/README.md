## Helm chart deploy for Elasticsearch 8.5.1 on AKS 
Ref: 
```
https://artifacthub.io/packages/helm/elastic/elasticsearch
```

## Deploy steps
```
kubectl create ns elastic
helm repo add elastic https://helm.elastic.co
helm repo update
helm -n elastic install elasticsearch-crew elastic/elasticsearch -f https://raw.githubusercontent.com/svdang999/ELK/main/elasticsearch/helm_elasticsearch_8_5_1/elasticsearch-8-5-1-custom-values.yaml
```
