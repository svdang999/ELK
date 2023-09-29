### Deployment 

```
https://artifacthub.io/packages/helm/elastic/elasticsearch/7.17.1
```

```
helm repo add elastic https://helm.elastic.co
helm -n logging install my-elasticsearch elastic/elasticsearch --version 7.17.1 -f https://raw.githubusercontent.com/svdang999/ELK/main/elasticsearch/helm_elasticsearch_7.17.1/custom-values.yaml
```

### Generate certificate steps

```
1. Create the p12
	elasticsearch@elasticsearch-master-0:~$ bin/elasticsearch-certutil cert -out -- elastic-certificates.p12 -pass ""
2. Copy the p12 to the local computer
	kubectl cp elasticsearch-master-0:elastic-certificates.p12 elastic-certificates.p12
3. Create a K8S Secret
	kubectl create secret generic elastic-certificates --from-file=elastic-certificates.p12
4. Stop Elasticsearch and Kibana
	helm uninstall elasticsearch
	equal with Kibana
5. Edit Elasticsearch values.yaml
	elasticsearch.yaml |
	    xpack.security.enabled: true
	    xpack.security.transport.ssl.enabled: true
	    xpack.security.transport.ssl.verification_mode: none 
	    xpack.security.http.ssl.verification_mode: none
	    xpack.security.transport.ssl.client_authentication: required
	    xpack.security.transport.ssl.keystore.path: /usr/share/elasticsearch/config/certs/elastic-certificates.p12
	    xpack.security.transport.ssl.truststore.path: /usr/share/elasticsearch/config/certs/elastic-certificates.p12
	    xpack.security.http.ssl.enabled: true
	    xpack.security.http.ssl.truststore.path: /usr/share/elasticsearch/config/certs/elastic-certificates.p12
	    xpack.security.http.ssl.keystore.path: /usr/share/elasticsearch/config/certs/elastic-certificates.p12 
	protocol: https
	secretMounts:
	  - name: elastic-certificates
	    secretName: elastic-certificates
	    path: /usr/share/elasticsearch/config/certs
	
6. Restart Elasticsearch
	helm install elasticsearch .
	
7. Set up Passwords
![image](https://github.com/svdang999/ELK/assets/101574223/627534d5-105e-4085-9012-55e3138d6281)

```
