# openssh-server
---

#### Interactive terminal when SSH
```bash
$ export TERM=ansi
```

#### SSH Configuration

1. Install `openssh-server`
```bash
$ su -
# apt install openssh-server -y
```

2. From client: `ssh SERVER_USERNAME@SERVER_IP`

#### Connect with user/client private key.
The process below occurs on the client. Not on the server.
1. Creating public & private key pair with ssh-keygen.
```bash
$ mkdir ssh_key && cd ssh_key
$ ssh-keygen -f name_file
$ ls -l
name_file
name_file.pub
$ ssh-copy-id -i name_file.pub SERVER_USERNAME@SERVER_IP
$ ssh SERVER_USERNAME@SERVER_IP -i name_file
```

#### What happen on server?
When client run `ssh-copy-id`, the server received client's public key and save it to `~/.ssh/authorized_keys`.