### 1. Install Elasticsearch 8.5.1
```
https://github.com/svdang999/ELK/blob/main/elasticsearch/helm_elasticsearch_8_5_1/README.md
```

### 2. Install Kibana version 8.5.1

```
https://artifacthub.io/packages/helm/elastic/kibana
```

```
helm install kibana-crew elastic/kibana --set env.ELASTICSEARCH_URL=https://elasticsearch-master.logging:9200 --namespace logging
```

### 3. Verify results
```
root@CPU016:~/GIT_Repos# helm -n logging list
NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
elasticsearch-crew      logging         7               2023-09-21 09:52:17.5749641 +0700 +07   deployed        elasticsearch-8.5.1     8.5.1
kibana-crew             logging         1               2023-09-21 13:16:37.1492642 +0700 +07   deployed        kibana-8.5.1            8.5.1
```


### Uninstall Kibana, then Elastic chart
```
helm -n logging uninstall kibana-crew
helm -n logging uninstall elasticsearch-crew
```
