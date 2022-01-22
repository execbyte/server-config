# Secure ProFTPd
---

#### Interactive terminal when SSH
```bash
$ export TERM=ansi
```

#### Proftpd basic
1. Install proftpd
```bash
$ su -
# apt install proftpd
```

2. Minimal config proftpd
```bash
# cd /etc/proftpd
# nano/vim proftpd.conf
```
- Comment `# UseIPv6	on` because not using IPv6.
- Uncomment `DefaultRoot	~`. It will fetch `/home/USER/` as ftp default folder when login.

3. Restart proftpd service
```bash
# systemctl restart proftdp.service
```

4. Test ftp from client
- If client is Linux ( Debian )
```bash
$ ftp tkj.com
Connected to tkj.com.
220 ProFTPD Server (tkj.com) [::ffff:IP_SERVER]
Name (tkj.com:qwerty): USERNAME_SERVER
331 Password required for USERNAME_SERVER
Password: 
230 User USERNAME_SERVER logged in
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 
```

- If windows using FileZilla

#### Secure ProFTPd
---

Use the certificate ( `server_apache-crt.crt` ) & private-key ( `server_apache.key` ) those were just made on Apache.

1. Make a folder as a place for certificate & private key.
```bash
# cd /etc/proftpd
# mkdir ssl-custom
```

2. Copy both of them from apache to proftpd.
```bash
# pwd
/etc/proftpd
# cp /etc/apache2/ssl-custom/server_apache* ./etc/proftpd/ssl-custom
```

3. Config `proftpd.conf`.
```bash
# nano/vim proftpd.conf
```
- Uncomment `Include /etc/proftpd/tls.conf`

4. Config `tls.conf`.
```bash
# nano/vim tls.conf
```
- Uncomment `TLSEngine		on`.
- Uncomment `TLSLog			/var/log/proftpd/tls.log`.
- Uncomment `TLSProtocol	TLSv1.2`.
- Uncomment `TLSRSACertificateFile`, change the value to where the certificate is stored `/etc/proftpd/ssl-custom/server_apache-crt.crt`.
- Uncomment `TLSRSACertificateKeyFile`, change the value to where the certificate is stored `/etc/proftpd/ssl-custom/server_apache.key`.
- Uncomment `TLSOptions`, change the value to `NoSessionReuseRequired`.

5. Restart proftpd service.
```bash
# systemctl restart proftpd.service
```