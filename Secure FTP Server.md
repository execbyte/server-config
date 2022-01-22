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
