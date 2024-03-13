# Bootstrap

This ansible playbook sets up what's described in the article ["My First 5 Minutes On A Server; Or, Essential Security for Linux Servers"](https://web.archive.org/web/20201112012219/https://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers). The ansible scripts are tested with [Ubuntu 22.04 Server](https://ubuntu.com/download/server).

## Prerequisites

### Unix environment
WSL on windows

### Ansible
A UNIX environment: https://docs.ansible.com/ansible/latest/installation_guide/index.html

### Python environemnt
The `netaddr` Python library needs to be installed on the control machine

### Kubernetes CLI
Install [kubectl](https://kubernetes.io/docs/tasks/tools/)

This should create a `~/.kube` directory, else:

```zsh
mkdir -p ~/.kube
```

### SSH key pairs
Run the ssh-keygen command. You can specify the type of key and the file in which to save the key with additional options, but for most users, the defaults are sufficient.
```zsh
ssh-keygen -t rsa -b 4096
```

Copy the public key to your managed node(s) 
To enable passwordless SSH authentication, you need to copy the public key (id_rsa.pub) to each managed node. The easiest way to do this is with the `ssh-copy-id` command:
```zsh
ssh-copy-id node@192.168.238.134
```

## Getting started

1. Edit personal information in `secrets.yml` with [`ansible-vault`](https://docs.ansible.com/ansible/latest/user_guide/vault.html) (default password: **password**)

   ```sh
   cp secrets.default secrets.yml
   ansible-vault rekey secrets.yml
   ansible-vault edit secrets.yml
   ```

2. Add your server by creating the `inventory` file with the following contents:

   ```ini
   [groupname]
   server.ip.or.hostname
   ```

   `[groupname]` can be omitted or altered to anything to your liking. You can have multiple groups. [Read more on the inventory.](https://docs.ansible.com/ansible/2.9/user_guide/intro_inventory.html)

3. Run `ansible-galaxy install -r requirements.yml` to install the dependencies.

4. Run (replace `<SUDOER>` with the login name)

   ```sh
   ansible-playbook -u node --private-key=~/.ssh/id_rsa --ask-become-pass --ask-vault-pass --inventory-file=inventory playbook.yml
   ```

### Initialisation

```zsh
ansible-playbook -i ansible/bootstrap/inventory/hosts.yml ansible/bootstrap/playbooks/initialise.yml --ask-become-pass --ask-vault-pass
```

## Included roles

### bootstrap

This is the baseline for every server and the initial reason these scripts exist.

- change the root password
- add an "admin" user (username of your choice)
- add your public key to `.ssh/authorized_keys` for the admin user
- add that admin user to `/etc/sudoers`
- install [ufw](https://launchpad.net/ufw) (uncomplicated firewall, an iptables frontend) and [fail2ban](https://www.fail2ban.org/)
- SSH: disallow password authentication
- SSH: disallow root login
- setup firewall to allow SSH traffic
- install additional apt packages provided by the user
- add a "deploy" user (username of your choice) _without sudo_ usage permission
- add your public key to the authorized keys of the deploy user
- setup periodic [unattended-upgrades](https://wiki.debian.org/UnattendedUpgrades) with automatic reboots
- setup [sSMTP](https://wiki.debian.org/sSMTP) to send (system) mails via SMTP
- create a 1GB file at `/swapfile` and swap on it
- set sysctl `vm.swappiness = 0`


