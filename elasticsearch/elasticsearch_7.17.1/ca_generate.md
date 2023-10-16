### Generates a CA certificate and private key in PKCS#12 format 
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
elasticsearch@elasticsearch-master-0:~$
elasticsearch@elasticsearch-master-0:~$
elasticsearch@elasticsearch-master-0:~$ ls -lsrht | grep elastic-stack-ca.p12
4.0K -rw-------  1 elasticsearch elasticsearch 2.7K Oct 16 09:21 elastic-stack-ca.p12
```
