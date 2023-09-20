### Helm chart deploy for Elasticsearch 8.5.1 on AKS 
Ref: 
```
https://artifacthub.io/packages/helm/elastic/elasticsearch
```

### Deploy steps
```
kubectl create ns elastic
helm repo add elastic https://helm.elastic.co
helm repo update
helm -n elastic install elasticsearch-crew elastic/elasticsearch -f https://raw.githubusercontent.com/svdang999/ELK/main/elasticsearch/helm_elasticsearch_8_5_1/elasticsearch-8-5-1-custom-values.yaml
```
### Verify results success
```
NAME: elasticsearch-crew
LAST DEPLOYED: Wed Sep 20 15:33:02 2023
NAMESPACE: elastic
STATUS: deployed
REVISION: 1
NOTES:
1. Watch all cluster members come up.
  $ kubectl get pods --namespace=elastic -l app=elasticsearch-master -w
2. Retrieve elastic user's password.
  $ kubectl get secrets --namespace=elastic elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d
3. Test cluster health using Helm test.
  $ helm --namespace=elastic test elasticsearch-crew
```
