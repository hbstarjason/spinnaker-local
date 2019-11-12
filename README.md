# Run Spinnaker on bare metal With docker-compose
You can run spinnaker on CentOS or Ubuntu by docker-compose.

## Quickstart
Provision a VM with at least 16GB memory and 4 cores.

```bash
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
$ git clone https://github.com/hbstarjason/spinnaker-local
$ cd spinnaker-local

# add k8s-v2-account
# kubectl config current-context

$ vi ./config/clouddriver.yml
context: XXXXXX

$ docker-compose up -d
```

## To access the spinnaker
Visit `http://localhost:9000` in browser if you run spinnaker on local machine.

Otherwise you can use ssh to create tunnel on local machine. Spinnaker exposes two ports, 9000 for web, 8084 for api gateway.
```bash
$ ssh -L 8084:localhost:8084 <remote-host>
$ ssh -L 9000:localhost:9000 <remote-host>
```

If your VM only private_key is allowed to log in , Please do the following.

```bash
$ vi ~/.ssh/config

# Configure as the output say in: vagrant ssh-config 

Host  <spinnaker-vm name>
  HostName localhost
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  ## Put your own identity file
  IdentityFile  <spinnaker-vm private_key>
  IdentitiesOnly yes
  LogLevel FATAL
  ControlMaster yes
  ControlPath ~/.ssh/spinnaker-tunnel.ctl
  RequestTTY no
  LocalForward 9000 localhost:9000
  LocalForward 8084 localhost:8084
  
$ ssh  <spinnaker-vm name>
```

