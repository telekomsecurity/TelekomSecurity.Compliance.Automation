# Telekom Security - Compliance Automation (Linux OS for Servers)

Company: [T-systems International GmbH](https://www.t-systems.com)

Author : [Markus Schumburg](mailto://security.automation@telekom.de)

Version: 0.9

Date   : 29. Nov 2018

-------------------------------------------------------------------------------

## Description

This Ansible role can be used to implement hardening of the Linux OS on
servers. The hardening will be done following the security requirements
from Telekom Security (see 'References' for used versions of document).

Note! It is an corresponding Ansible role available to check compliance
      to security requirements from Telekom Security for Linux Server OS.


## Platforms

The role is tested with the following OpenSSH version:
<tbd>


## Structure


## Inventory

Ansible by default uses the `/etc/ansible/hosts` file to specify hosts and
needed parameters. To use your own 'hosts' file use

```
$ ansible-playbook -i <location-of-host-file> <playbook>.yml
```

This is fine for testing purposes. In case of productive environments a dynamic
inventory triggered by the used orchestration should be used. For more
information see:


* http://docs.ansible.com/ansible/latest/intro_inventory.html
* http://docs.ansible.com/ansible/latest/intro_dynamic_inventory.html


## Variables


## Execute

Ansible is agent-free. This means no agent is needed on systems that should be
configured with Ansible. But Ansible uses python. Python must be installed on
systems to be managed with Ansible!

Ansible uses SSH to connect to remote systems. To connect and to perform all
tasks a user is needed on the system that should be hardened. This user needs
root rights and must be a member of sudo group. Needed parameters for the user
can be defined in inventory or playbook file.

Check for user parameters:
http://docs.ansible.com/ansible/latest/intro_inventory.html

------------------------------------------------------------------------------

**IMPORTANT:** Don't use user 'root' to execute this role. The role will disable
local and remote login via SSH for user 'root'! Create your own user with root
rights and sudo group membership.

-------------------------------------------------------------------------------

The default path to store roles is `/etc/ansible/roles`. In the file
`/etc/ansible/ansible.cfg` with variable `roles_path` an own path can be
specified.

Start playbook with:

```
$ ansible-playbook <playbook>.yml
```

## References

Telekom Security - Security Requirements:
* SecReq 3.04: Secure Shell (SSH)

## License

Apache License, Version 2.0
See file LICENSE
