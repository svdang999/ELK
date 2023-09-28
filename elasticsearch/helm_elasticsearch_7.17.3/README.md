### Deployment 

```
https://artifacthub.io/packages/helm/elastic/elasticsearch/7.17.3
```

```
helm repo add elastic https://helm.elastic.co
helm -n logging install my-elasticsearch elastic/elasticsearch --version 7.17.3 -f https://raw.githubusercontent.com/svdang999/ELK/main/elasticsearch/helm_elasticsearch_7.17.3/custom-values.yaml
```
