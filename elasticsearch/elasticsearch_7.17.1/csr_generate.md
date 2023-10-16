### 1. Generate a Certificate Signing Request (CSR)

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

Generate a CSR? [y/N]y

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

node #1 name: aks-crew-test-node1

## Which hostnames will be used to connect to aks-crew-test-node1?

These hostnames will be added as "DNS" names in the "Subject Alternative Name"
(SAN) field in your certificate.

You should list every hostname and variant that people will use to connect to
your cluster over http.
Do not list IP addresses here, you will be asked to enter them later.

If you wish to use a wildcard certificate (for example *.es.example.com) you
can enter that here.

Enter all the hostnames that you need, one per line.
When you are done, press <ENTER> once more to move on to the next step.

http://kibana-crew-7-kibana.logging
kibana-crew-7-kibana:5601
http://kibana-crew-7-kibana

You entered the following hostnames.

 - http://kibana-crew-7-kibana.logging
 - kibana-crew-7-kibana:5601
 - http://kibana-crew-7-kibana

Is this correct [Y/n]Y

## Which IP addresses will be used to connect to aks-crew-test-node1?

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

Key Name: aks-crew-test-node1
Subject DN: CN=aks-crew-test-node1
Key Size: 2048

Do you wish to change any of these options? [y/N]N
Generate additional certificates? [Y/n]n

## What password do you want for your private key(s)?

Your private key(s) will be stored as a PEM formatted file.
We recommend that you protect your private keys with a password

If you do not wish to use a password, simply press <enter> at the prompt below.
Provide a password for the private key:  [<ENTER> for none]

## Where should we save the generated files?

A number of files will be generated including your private key(s),
certificate request(s), and sample configuration options for Elastic Stack products.

These files will be included in a single zip archive.

What filename should be used for the output zip file? [/usr/share/elasticsearch/elasticsearch-ssl-http.zip]

Zip file written to /usr/share/elasticsearch/elasticsearch-ssl-http.zip
elasticsearch@elasticsearch-master-0:~$
```

### 2. Copy the generated zip file to the local computer
```
kubectl -n logging cp elasticsearch-master-0:/usr/share/elasticsearch/elasticsearch-ssl-http.zip elasticsearch-ssl-http.zip
```
