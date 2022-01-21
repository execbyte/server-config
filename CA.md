# Certificate Authority
---

#### Source
1. [local-site-generator](https://github.com/dakshshah96/local-cert-generator)

#### Interactive terminal when SSH
```bash
$ export TERM=ansi
```

#### Debian as CA 
1. Become CA.
```bash
$ cd ~/
$ mkdir CA-local && cd CA-local
$ openssl genrsa -des3 -out rootCA_private-key.key 2048
$ openssl req -x509 -new -nodes -key rootCA_private-key.key -sha256 -days 7300 -out rootCA_cert.pem
```

2. Prepare the config-CSR ( Certificate Signing Request ) file.
```bash
$ touch server-apache-csr.cnf
$ nano/vim server-apache-csr.cnf
[req]
default_bits = 2048
prompt = no
default_md = sha256
distinguished_name = dn

[dn]
C=ID
ST=INDONESIA
L=JEMBER
O=INSTITUTE TECHNOLOGI SMK 3 TANGGUL
OU=FAKULTAS TEKNOLOGI JARINGAN KOMPUTER
emailAddress=admintkj@smk3.com
CN = tkj.com
```

3. Create CSR file from config-CSR file.
```bash
$ openssl req -new -sha256 -nodes -out server_apache.csr -newkey rsa:2048 -keyout server_apache_private-key.key -config server-apache-csr.cnf
```

4. Create v3 extended file.
```bash
$ touch v3.ext
$ nano/vim v3.ext
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = tkj.com
```

5. Request to CA ( Debian ) with CSR + v3 extended file. The CA ( Debian ) will sign it with his private-key + certificate ( \*.pem ) file.
```bash
$ openssl x509 -req -in server_apache.csr -CA rootCA_cert.pem -CAkey rootCA_private-key.key -CAcreateserial -out server_apache-crt.crt -days 825 -sha256 -extfile v3.ext
$ ls -l
rootCA_cert.pem
rootCA_cert.srl
rootCA_private-key.key
server_apache-crt.crt
server_apache-csr.cnf
server_apache.csr
server_apache_private-key.key
v3.ext
```
