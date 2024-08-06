1. Deploy Elastic
```
kubectl -n logging apply -f operator-secrets.yaml
helm -n logging install elasticsearch-7 elastic/elasticsearch --version 7.17.1 --set replicas=1 --set minimumMasterNodes=1 --set volumeClaimTemplate.resources.requests.storage=64Gi
```

2. Generates a CA certificate and private key in PKCS#12 format
```
elasticsearch@elasticsearch-master-0:~$ bin/elasticsearch-certutil ca
This tool assists you in the generation of X.509 certificates and certificate
signing requests for use with SSL/TLS in the Elastic stack.

The 'ca' mode generates a new 'certificate authority'
This will create a new X.509 certificate and private key that can be used
to sign certificate when running in 'cert' mode.

Use the 'ca-dn' option if you wish to configure the 'distinguished name'
of the certificate authority

By default the 'ca' mode produces a single PKCS#12 output file which holds:
    * The CA certificate
    * The CA's private key

If you elect to generate PEM format certificates (the -pem option), then the output will
be a zip file containing individual files for the CA certificate and private key

Please enter the desired output file [elastic-stack-ca.p12]:
Enter password for elastic-stack-ca.p12 :
elasticsearch@elasticsearch-master-0:~$ ls -lsrht | grep elastic-stack-ca.p12
4.0K -rw-------  1 elasticsearch elasticsearch 2.7K Aug  6 07:55 elastic-stack-ca.p12
```




3. Generate a Certificate Signing Request using elastic-stack-ca.p12 created in step 2
```
elasticsearch@elasticsearch-master-0:~$ ./bin/elasticsearch-certutil http

## Elasticsearch HTTP Certificate Utility

The 'http' command guides you through the process of generating certificates
for use on the HTTP (Rest) interface for Elasticsearch.

This tool will ask you a number of questions in order to generate the right
set of files for your needs.

## Do you wish to generate a Certificate Signing Request (CSR)?

A CSR is used when you want your certificate to be created by an existing
Certificate Authority (CA) that you do not control (that is, you don't have
access to the keys for that CA).

If you are in a corporate environment with a central security team, then you
may have an existing Corporate CA that can generate your certificate for you.
Infrastructure within your organisation may already be configured to trust this
CA, so it may be easier for clients to connect to Elasticsearch if you use a
CSR and send that request to the team that controls your CA.

If you choose not to generate a CSR, this tool will generate a new certificate
for you. That certificate will be signed by a CA under your control. This is a
quick and easy way to secure your cluster with TLS, but you will need to
configure all your clients to trust that custom CA.

Generate a CSR? [y/N]N

## Do you have an existing Certificate Authority (CA) key-pair that you wish to use to sign your certificate?

If you have an existing CA certificate and key, then you can use that CA to
sign your new http certificate. This allows you to use the same CA across
multiple Elasticsearch clusters which can make it easier to configure clients,
and may be easier for you to manage.

If you do not have an existing CA, one will be generated for you.

Use an existing CA? [y/N]y

## What is the path to your CA?

Please enter the full pathname to the Certificate Authority that you wish to
use for signing your new http certificate. This can be in PKCS#12 (.p12), JKS
(.jks) or PEM (.crt, .key, .pem) format.
CA Path: /usr/share/elasticsearch/elastic-stack-ca.p12
Reading a PKCS12 keystore requires a password.
It is possible for the keystore's password to be blank,
in which case you can simply press <ENTER> at the prompt
Password for elastic-stack-ca.p12:

## How long should your certificates be valid?

Every certificate has an expiry date. When the expiry date is reached clients
will stop trusting your certificate and TLS connections will fail.

Best practice suggests that you should either:
(a) set this to a short duration (90 - 120 days) and have automatic processes
to generate a new certificate before the old one expires, or
(b) set it to a longer duration (3 - 5 years) and then perform a manual update
a few months before it expires.

You may enter the validity period in years (e.g. 3Y), months (e.g. 18M), or days (e.g. 90D)

For how long should your certificate be valid? [5y] 10y

## Do you wish to generate one certificate per node?

If you have multiple nodes in your cluster, then you may choose to generate a
separate certificate for each of these nodes. Each certificate will have its
own private key, and will be issued for a specific hostname or IP address.

Alternatively, you may wish to generate a single certificate that is valid
across all the hostnames or addresses in your cluster.

If all of your nodes will be accessed through a single domain
(e.g. node01.es.example.com, node02.es.example.com, etc) then you may find it
simpler to generate one certificate with a wildcard hostname (*.es.example.com)
and use that across all of your nodes.

However, if you do not have a common domain name, and you expect to add
additional nodes to your cluster in the future, then you should generate a
certificate per node so that you can more easily generate new certificates when
you provision new nodes.

Generate a certificate per node? [y/N]y

## What is the name of node #1?

This name will be used as part of the certificate file name, and as a
descriptive name within the certificate.

You can use any descriptive name that you like, but we recommend using the name
of the Elasticsearch node.

node #1 name: aks-operator-prd-node01

## Which hostnames will be used to connect to aks-operator-prd-node01?

These hostnames will be added as "DNS" names in the "Subject Alternative Name"
(SAN) field in your certificate.

You should list every hostname and variant that people will use to connect to
your cluster over http.
Do not list IP addresses here, you will be asked to enter them later.

If you wish to use a wildcard certificate (for example *.es.example.com) you
can enter that here.

Enter all the hostnames that you need, one per line.
When you are done, press <ENTER> once more to move on to the next step.

kibana-operator-7-kibana
kibana-operator-7-kibana.elastic
kibana-operator-7-kibana.elastic:5601
http://kibana-operator-7-kibana
http://kibana-operator-7-kibana.elastic
kibana-7-kibana
kibana-7-kibana.logging
kibana-7-kibana:5601
http://kibana-7-kibana
http://kibana-7-kibana.logging

You entered the following hostnames.

 - kibana-operator-7-kibana
 - kibana-operator-7-kibana.elastic
 - kibana-operator-7-kibana.elastic:5601
 - http://kibana-operator-7-kibana
 - http://kibana-operator-7-kibana.elastic
 - kibana-7-kibana
 - kibana-7-kibana.logging
 - kibana-7-kibana:5601
 - http://kibana-7-kibana
 - http://kibana-7-kibana.logging

Is this correct [Y/n]Y

## Which IP addresses will be used to connect to aks-operator-prd-node01?

If your clients will ever connect to your nodes by numeric IP address, then you
can list these as valid IP "Subject Alternative Name" (SAN) fields in your
certificate.

If you do not have fixed IP addresses, or not wish to support direct IP access
to your cluster then you can just press <ENTER> to skip this step.

Enter all the IP addresses that you need, one per line.
When you are done, press <ENTER> once more to move on to the next step.


You did not enter any IP addresses.

Is this correct [Y/n]Y

## Other certificate options

The generated certificate will have the following additional configuration
values. These values have been selected based on a combination of the
information you have provided above and secure defaults. You should not need to
change these values unless you have specific requirements.

Key Name: aks-operator-prd-node01
Subject DN: CN=aks-operator-prd-node01
Key Size: 2048

Do you wish to change any of these options? [y/N]N
Generate additional certificates? [Y/n]n

## What password do you want for your private key(s)?

Your private key(s) will be stored in a PKCS#12 keystore file named "http.p12".
This type of keystore is always password protected, but it is possible to use a
blank password.

If you wish to use a blank password, simply press <enter> at the prompt below.
Provide a password for the "http.p12" file:  [<ENTER> for none]

## Where should we save the generated files?

A number of files will be generated including your private key(s),
public certificate(s), and sample configuration options for Elastic Stack products.

These files will be included in a single zip archive.

What filename should be used for the output zip file? [/usr/share/elasticsearch/elasticsearch-ssl-http.zip]

Zip file written to /usr/share/elasticsearch/elasticsearch-ssl-http.zip
elasticsearch@elasticsearch-master-0:~$
```



3.2  Copy the generated zip file to the local computer
```
kubectl -n logging cp elasticsearch-master-0:/usr/share/elasticsearch/elasticsearch-ssl-http.zip elasticsearch-ssl-http.zip
```

```
$ kubectl -n logging cp elasticsearch-master-0:/usr/share/elasticsearch/elasticsearch-ssl-http.zip elasticsearch-ssl-http.zip
Defaulted container "elasticsearch" out of: elasticsearch, configure-sysctl (init)
tar: Removing leading `/' from member names
```




4. Create Elastic Secret
```
kubectl -n logging create secret generic elastic-certificates --from-file="C:\tmp\temp_files\Repos\dev_temp\elasticsearch-ssl-http\elasticsearch\http.p12"
secret/elastic-certificates created
```

5. Upgrade Elastic
```
helm -n logging upgrade elasticsearch-7 elastic/elasticsearch --version 7.17.1 --set replicas=1 --set minimumMasterNodes=1 -f operator-elasticsearch-custom-values.yaml
```

6. Deploy Kibana
```
helm -n logging install kibana-7 elastic/kibana --set env.ELASTICSEARCH_URL=http://elasticsearch-master:9200 --version 7.17.1 -f operator-kibana-custom-values.yaml
```


### Optional
### Setup passwords Elastic 
```
./bin/elasticsearch-setup-passwords interactive
elastic / password-here
```

```
elasticsearch@elasticsearch-master-0:~$ ./bin/elasticsearch-setup-passwords interactive
Initiating the setup of passwords for reserved users elastic,apm_system,kibana,kibana_system,logstash_system,beats_system,remote_monitoring_user.
You will be prompted to enter passwords as the process progresses.
Please confirm that you would like to continue [y/N]y


Enter password for [elastic]:
Reenter password for [elastic]:
Enter password for [apm_system]:
Reenter password for [apm_system]:
Enter password for [kibana_system]:
Reenter password for [kibana_system]:
Enter password for [logstash_system]:
Reenter password for [logstash_system]:
Enter password for [beats_system]:
Reenter password for [beats_system]:
Enter password for [remote_monitoring_user]:
Reenter password for [remote_monitoring_user]:
Changed password for user [apm_system]
Changed password for user [kibana_system]
Changed password for user [kibana]
Changed password for user [logstash_system]
Changed password for user [beats_system]
Changed password for user [remote_monitoring_user]
Changed password for user [elastic]
elasticsearch@elasticsearch-master-0:~$
```
