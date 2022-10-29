# Node Ansible

Ansible playbooks to setup epns node.

### Requirements

Make sure you are using python3.x with Ansible. To check: `ansible --version` 

### Setup

Note: If your ssh public key (`~/.ssh/id_rsa.pub`) is already on the remote machines, skip this step.

**Copy `pem` private key file as `.workspace/private.pem`** to enable ssh through ansible. If you don't have pem file, just make sure you can reach remote machines from your own machine using ssh (`ssh <username>@ip`). 

### Inventory

Ansible manages hosts using `inventory.yml` file. Current setup has two group names:

* `geth`

Each group may have multiple IPs (hosts). Each Ansible command needs group name to be mentioned. Ansible runs the playbook on mentioned group's machines. Note that if you don't mention group name, Ansible will run playbook on all machines.

**Setup inventory**

Add sentry nodes IPs/hosts under the `geth` group

Example:

```yml
all:
  hosts:
  children:
    geth:
      hosts:
        xxx.xxx.xx.xx: # <----- Add IP for geth node
        xxx.xxx.xx.xx: # <----- Add IP for geth node
```

Note: By default the user to login is setup as ubuntu in `group_vars/all` file. If you have a specific user to be logged in with please change the username in this file.

To check if nodes are reachable, run following commands:

```bash
# to check if sentry nodes are reachable
ansible geth -m ping

```

### geth node setup

To show list of hosts where the playbook will run (notice `--list-hosts` at the end):

```bash
ansible-playbook -l geth playbooks/main.yml  --list-hosts
```

To run actual playbook on geth nodes:

```bash
ansible-playbook -l geth playbooks/main.yml
```

### Adhoc queries

**Ping**

Just to see if machines are reachable:

For geth nodes:

```bash
ansible geth -m ping
```

`ping` is a module name. You can any module and arguments here.

**Run shell command**

Following command will fetch and print all disk space stats from all `geth` hosts.

For geth nodes:

```bash
ansible geth -m shell -a "df -h"
```
