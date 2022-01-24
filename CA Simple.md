# CA - Simple
---

#### Interactive terminal when SSH
```bash
$ export TERM=ansi
```

#### Debian as CA
1. Become CA. Creating CA private key.
```bash
$ openssl genpkey -algorithm RSA -out CA.key -pkeyopt rsa_keygen_bits:4096
```
or with more simple command.
```bash
$ openssl genrsa -out CA.key 2048
```

> genpkey / genrsa: generated private key

2. Create CA certificate with newly generated private key.
```bash
$ openssl req -x509 -key CA.key -days 3650 -out CA.pem
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:ID
State or Province Name (full name) [Some-State]:INDONESIA
Locality Name (eg, city) []:JEMBER
Organization Name (eg, company) [Internet Widgits Pty Ltd]:SMK 3 TANGGUL
Organizational Unit Name (eg, section) []:TKJ
Common Name (e.g. server FQDN or YOUR name) []:smk3.com
Email Address []:admin@smk3.com
```

> req -x509: generated certificate
> -key: CA private key
> -days: 365 * 10 = 3650 days
> -out: certificate output in pem format

3. Creating CSR for apache.
```bash
$ openssl req -new -out apache.csr -keyout apache.key
Generating a RSA private key
......+++++
..............................................................................................+++++
writing new private key to 'apache.key'
Enter PEM pass phrase:1234
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:ID
State or Province Name (full name) [Some-State]:INDONESIA
Locality Name (eg, city) []:JEMBER
Organization Name (eg, company) [Internet Widgits Pty Ltd]:SMK 3 TANGGUL
Organizational Unit Name (eg, section) []:TKJ
Common Name (e.g. server FQDN or YOUR name) []:tkj.com
Email Address []:admin@tkj.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

> Enter PEM pass phrase: 4 digit password
> req -new: creating new request
> -out: csr output
> -keyout: private key output

4. Create v3 extended file.
```bash
$ touch v3.ext
$ nano/vim v3.ext
subjectAltName = @alt_names

[alt_names]
DNS.1 = tkj.com
DNS.2 = mail.tkj.com
DNS.3 = smk3.sch.id
DNS.4 = blog.rizki.com
```

> DNS.n: This is called Subject Alternative Name ( SAN ).
> SAN is useful for many domains & subdomains in 1 certificate file.

5. Request to CA ( Debian ) with CSR. The CA ( Debian ) will sign it with his private-key + certificate ( *.pem ) file.
```bash
$ openssl x509 -req -in apache.csr -CA CA.pem -CAkey CA.key -CAcreateserial -out apache.crt -days 365 -extfile v3.ext
Signature ok
subject=C = ID, ST = INDONESIA, L = JEMBER, O = SMK 3 TANGGUL, OU = TKJ, CN = *.tkj.com, emailAddress = admin@tkj.com
Getting CA Private Key

$ ls -l
apache.crt
apache.csr
apache.key
CA.key
CA.pem
CA.srl
```

> x509 -req: sign new request of certificate by CA
> -in: input the csr request
> -CA: input the CA certificate ( CA.pem )
> -CAkey: input the CA private key ( CA.key )
> -CAcreateserial: creating serial file. default needed. ( CA.srl )
> -out: new certificate from csr which one has been signed ( apache.crt )
> -days: 365 days ( 1 year ) less than CA days ( 10 years )
> -extfile: include the SAN file.