#Netapp SSH Key Rotation

Rotating SSH keys on a NetApp storage array using Ansible involves a few steps, and you'll need to follow a structured process to update the keys while ensuring minimal disruption to your environment. Here's an outline of the process, followed by the Ansible tasks to achieve it.

###Prerequisites
- NetApp Ansible Collection: Ensure you have the netapp.ontap Ansible collection installed. You can install it using:

ansible-galaxy collection install netapp.ontap

- Access to NetApp: Ensure you have SSH access to the NetApp storage array and the necessary administrative privileges to update the SSH keys.
- Backup Existing Configuration: Always ensure that you back up the current SSH configuration before making changes.
- Ansible Vault (Optional): For securely handling sensitive data such as private keys, you may want to use Ansible Vault to encrypt variables.

###Steps to Rotate SSH Keys
- Generate New SSH Key Pair

First, generate a new SSH key pair on the machine where you'll be running Ansible, if you haven’t already.
ssh-keygen -t rsa -b 2048 -f ~/.ssh/new_netapp_key
You'll need the public key (~/.ssh/new_netapp_key.pub) for copying to the NetApp storage array.

- Prepare Ansible Playbook

The main steps in the Ansible playbook are:

Disable the old SSH key on the NetApp device.
Deploy the new public SSH key to the NetApp device.
Enable the new SSH key (if applicable, depending on the device's configuration).
Clean up old keys if necessary.

###Key Points to Understand:

- na_ssh_key Module: This module is part of the netapp.ontap collection and is used to manage SSH keys. The state: present ensures that the SSH key is added, while state: absent will remove the key.
- Secure Password Management: I’ve used lookup('env', 'NETAPP_PASSWORD') to fetch the password from the environment variable, but it’s highly recommended to store passwords securely, using Ansible Vault, for example.
- Key Removal: The playbook retrieves the list of existing keys and removes them if necessary before adding the new key.


