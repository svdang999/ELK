### Deploy Elasticsearch 7.17.1

```
helm repo add elastic https://helm.elastic.co
helm repo update
helm -n elastic install elastic-crew-7 elastic/elasticsearch --version 7.17.1 --set replicas=1 --set minimumMasterNodes=1
```

### Deploy Kibana 7.17.1
```
helm -n elastic install kibana-crew-7 elastic/kibana --set env.ELASTICSEARCH_URL=http://elasticsearch-master:9200 --version 7.17.1
```


### Upgrade chart
```
helm -n elastic upgrade elastic-crew-7 elastic/elasticsearch --version 7.17.1 --set replicas=1 --set minimumMasterNodes=1
```

### Uninstall
```
helm -n elastic uninstall kibana-crew
helm -n elastic uninstall elasticsearch-crew
```
