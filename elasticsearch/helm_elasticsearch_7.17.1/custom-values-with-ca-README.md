### Deployment 
```
https://artifacthub.io/packages/helm/elastic/elasticsearch/7.17.1
```

### Setup HTTPS/Certificate/Authentication for Elasticsearch  
1. Install Elastic basic (Without any authentication/SSL/HTTPS)
```
helm repo add elastic https://helm.elastic.co
helm repo update
helm -n logging install elasticsearch-crew-7 elastic/elasticsearch --version 7.17.1 --set replicas=1 --set minimumMasterNodes=1
```

2. Create & Copy the p12 to the local computer
```
https://github.com/svdang999/ELK/blob/main/elasticsearch/elasticsearch_7.17.1/csr_generate_with_exists_ca.md
```

3. Create a K8S Secrets
```
kubectl -n logging create secret generic elastic-certificates --from-file=elastic-certificates.p12
kubectl -n logging apply -f https://raw.githubusercontent.com/svdang999/ELK/main/elasticsearch/helm_elasticsearch_7.17.1/secrets.yaml
```
	
4. Upgrade Elasticsearch
```
helm -n logging upgrade elasticsearch-crew-7 elastic/elasticsearch --version 7.17.1 -f https://raw.githubusercontent.com/svdang999/ELK/main/elasticsearch/helm_elasticsearch_7.17.1/custom-values-with-auth.yaml
```
