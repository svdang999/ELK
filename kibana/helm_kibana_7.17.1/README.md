#### Deployment Kibana using Helm chart 3
```
helm -n logging install kibana-crew-7 elastic/kibana --version 7.17.1 -f https://raw.githubusercontent.com/svdang999/ELK/main/kibana/helm_kibana_7.17.1/crew-demo-custom-values.yaml
```

### Elasticsearch 7 deployment
```
https://github.com/svdang999/ELK/tree/main/elasticsearch/helm_elasticsearch_7.17.1
```
