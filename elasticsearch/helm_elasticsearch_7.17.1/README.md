### Deployment 

```
https://artifacthub.io/packages/helm/elastic/elasticsearch/7.17.1
```

```
helm repo add elastic https://helm.elastic.co
helm -n logging install my-elasticsearch elastic/elasticsearch --version 7.17.1 -f https://raw.githubusercontent.com/svdang999/ELK/main/elasticsearch/helm_elasticsearch_7.17.1/custom-values.yaml
```

### Generate certificate steps
### Ref 
```
https://discuss.elastic.co/t/im-struggling-set-up-the-minimal-security-and-the-configure-the-tls/321851
```

1. Create the p12
```
elasticsearch@elasticsearch-master-0:~$bin/elasticsearch-certutil cert -out elastic-certificates.p12 -pass ""
ls -lsrht /usr/share/elasticsearch/elastic-certificates.p12
```
3. Copy the p12 to the local computer
```
kubectl -n logging cp elasticsearch-master-0:/usr/share/elasticsearch/elastic-certificates.p12 elastic-certificates.p12

PS C:\Users\son.dang> ls | findstr.exe "elastic"
-a----         9/29/2023   7:19 PM           3596 elastic-certificates.p12
```

4. Create a K8S Secret
```
kubectl -n logging create secret generic elastic-certificates --from-file=elastic-certificates.p12
secret/elastic-certificates created
```
5. Stop Elasticsearch and Kibana
```
helm -n logging uninstall kibana
helm -n logging uninstall elasticsearch
```

6. Edit Elasticsearch values.yaml
```
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
```
	
7. Restart Elasticsearch
```
helm -n logging install elasticsearch-crew-7 elastic/elasticsearch --version 7.17.1 -f https://raw.githubusercontent.com/svdang999/ELK/main/elasticsearch/helm_elasticsearch_7.17.1/custom-values-with-auth.yaml
```
	
8. Set up Passwords
```
bin/elasticsearch-setup-passwords auto
```
9. Create password secret for user elastic/kibana 
```
kubectl -n logging apply -f secret.yaml
```
