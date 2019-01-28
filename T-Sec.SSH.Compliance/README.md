# T-Sec Compliance Automation for SSH

Company: [T-systems International GmbH](https://www.t-systems.com)

Author: [Telekom Security](https://security.telekom.com/)   

E-Mail: [security.automation@telekom.de](security.automation@telekom.de)

Version: 0.9

Date: 28. Jan 2019

-------------------------------------------------------------------------------

## Description

This Ansible role can be used to implement hardening of the Linux OS on
servers. The hardening will be done following the security requirements
from Telekom Security (see 'References' for used versions of document).

## Supported Platforms

Ansible version: 2.7.x
Python version: 2.7.x

The role is tested with the following SSH versions:

  * OpenSSH > Version 7.x

-------------------------------------------------------------------------------

**IMPORTANT:** The role should also work with older OpenSSH versions. But these are possibly outdated and should be updated to a newer version!

-------------------------------------------------------------------------------

## Preconditions to use Ansible

Ansible is agent-free. This means no agent is needed on systems that should be
configured with Ansible. But Ansible uses python. Python must be installed on
systems to be managed with Ansible!

Ansible uses SSH to connect to remote systems. To connect and to perform all
tasks a user is needed on the system that should be hardened. This user needs
root privileges and must be member of sudo group. Needed parameters for the user
can be defined in inventory or playbook file (see next chapter).

------------------------------------------------------------------------------

**IMPORTANT:** Don't use user 'root' to execute this role. The role will disable
local and remote login via SSH for user 'root'! Create your own user with root
rights and sudo group membership.

-------------------------------------------------------------------------------

## Inventory

Ansible by default uses the `/etc/ansible/hosts` file to specify hosts and
needed parameters.

To use your own 'hosts' file use:
```
$ ansible-playbook -i <location-of-host-file> <playbook>.yml
```

Example of 'hosts' file:
```
[test-system]
test ansible_host=127.0.0.1

[test-system:vars]
ansible_port=2222
ansible_user=vagrant
ansible_ssh_pass=vagrant
```

This is fine for testing purposes. In case of productive environments a dynamic
inventory triggered by the used orchestration should be used.

More information:
* [Intro to Ansible inventory](http://docs.ansible.com/ansible/latest/intro_inventory.html)
* [Dynamic Inventory](http://docs.ansible.com/ansible/latest/intro_dynamic_inventory.html)

## Role

The downloaded role must be stored in the directory for Ansible roles on your computer. The default path to store roles is `/etc/ansible/roles`. In the file
`/etc/ansible/ansible.cfg` with variable `roles_path` an own path can be
specified.

```
roles_path    = ~/roles
```

## Variables

The default variables are located in the file '/default/main.yml'. Before
execution of the playbook please change this variables following the demands
of the systems you like to harden.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| 'config_ipv6_enable' | false | Enables if IPv6 should be used. |
| 'config_mgmt_interface_ipv4' | false | Specifies if a dedicated IPv4 management interface is used. |
| 'mgmt_interface_ipv4' | 0.0.0.0 | Defines IPv4 address of management interface. |
| 'config_mgmt_interface_ipv6' | false | Enable if (true) a dedicated IPv6 management interface is used. |
| 'mgmt_interface_ipv6' | :: | Defines IPv6 address for management interface. |
| 'config_new_user' | false | Defines if a new user should be configured. |
| 'user_name' | none | Defines name for new user. |
| 'password' | none | Defines password for new user. |
| 'public_key_file' | {{ role_path }}/files/id_rsa.pub |  Defines path to public-key file (e.g. id_rsa.pub). |
| 'group_sudo' | sudo | Defines group used for sudo. |

Additional variables are located in the following files in directory '/vars':

* /vars
  * /main.yml
  * /RedHat.yml
  * /Ubuntu.yml
  * /vars_ssh(01)ssh-requirements.yml

-------------------------------------------------------------------------------

**NOTE:** Changing variables in these files can affect security compliance and must be approved by your responsible Project Security Manager!

-------------------------------------------------------------------------------

## Execution of Playbook

Example of playbook:
```
---

- hosts: test-system
  become: true    # Become root (sudo)
  roles:
    - T-Sec.LinuxOS.Compliance
```

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
