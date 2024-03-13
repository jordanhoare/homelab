
# Vault

A big crux of mine is managing the ability to regularly destroying and re-initlising new working machines using versioned controlled [dotfiles](https://github.com/jordanhoare/dotfiles) for both Windows and UNIX-based machines. 

While most of my infrastructure and workloads of the homelab are self-hosted I do rely upon the cloud for certain key parts of my setup to avoid having to deal with chicken and the egg scenarios.


## Goals
- Use a cloud vendored solution for maintain SSH keys
- Implement a second layer of security to my machines with key rotations and authentication


### Passwordless SSH
Before you can use Ansible, you need to set up passwordless SSH access to your target machines since Ansible relies on SSH for communication.

By default, Ubuntu installations don't use passwordless SSH (only newer versions enable the SSH server at all to begin with).

### Writing a customised preseeding Ubuntu ISO
Creating a custom Ubuntu ISO with your public SSH key and pre-seeded configurations included.

Using an Ubuntu installation (or flash) that includes a pre-seeded configuration (preseed.cfg / autoinstall). This can automate user creation and package installation during the OS installation process. 

### HashiCorp Vault
[HashiCorp Vault](https://www.vaultproject.io/) provides a centralised place to store your sensitive data, including SSH private keys. By keeping your private keys in Vault, you reduce the risk associated with storing them directly on your local machine or scattered across your infrastructure.



[Automatic install of Ubuntu server](https://www.youtube.com/watch?v=DtXZ6BMaKbA)

cat ~/.ssh/id_rsa.pub | ssh user@123.45.67.89 "cat >> ~/.ssh/authorized_keys"

### Step 1: Plan Your Playbook
Your playbook will need to:

Install any necessary packages (like vault-ssh-helper on the nodes if using Vault for SSH).
Configure your Ubuntu nodes to authenticate SSH connections using Vault.
Possibly adjust SSHD configuration to use the Vault-signed keys.

### Step 2: Authentication with Vault
Your playbook should include a task to authenticate with the HashiCorp Vault service. This can be done using the Vault CLI or API calls within Ansible. You might need a Vault token or other credentials stored securely in Ansible Vault or as GitHub Secrets if you're triggering this via GitHub Actions.

### Step 3: Vault Configuration for SSH
Since you're using Vault's hosted service, ensure your Vault is configured for SSH access. This might involve setting up an SSH Secret Engine and roles through the Vault UI or API.

### Dynamic 
Install and Configure Vault: Set up a Vault server and configure its SSH Secrets Engine.

Adjust Your Automation: Modify your Ansible playbooks or scripts to retrieve SSH credentials from Vault dynamically instead of using static keys stored on disk.

Policy and Access Management: Define policies in Vault that control access to the SSH keys, ensuring that only authorized users or systems can retrieve them.

Audit and Rotate Secrets: Use Vault's capabilities to audit access and rotate SSH keys regularly, enhancing security.