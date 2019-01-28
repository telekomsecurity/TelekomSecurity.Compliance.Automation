# T-Sec Compliance Automation for Linux OS for Servers

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

The role is tested with the following Linux versions:

  * Ubuntu 14.04 LTS
  * Ubuntu 16.04 LTS
  * Ubuntu 18.04 LTS
  * RedHat Enterprise Linux 7.x
  * CentOS 7.x
  * Oracle Linux 7.x
  * Amazon Linux 2

-------------------------------------------------------------------------------

**IMPORTANT:** This role only supports Linux versions for SERVERS! The role
is not tested with desktop systems and can cause unexpected malfunctions.

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
| 'config_iptables_rules_tcp' | true | Defines if TCP iptables rules should be configured. |
| 'tcp_services' | 22 | Defines needed TCP ports.|
| 'config_iptables_rules_udp' | true | Defines if UDP iptables rules should be configured. |
| 'tcp_services' | none | Defines needed UDP ports.|
| 'config_partitions' | false | Defines if separate partitions are used. |
| 'partitions' | tmp, var | Defines existing partitions. |
| 'config_ipv6_enable' | false | Enables if IPv6 should be used. |
| 'do_system_upgrade' | false | Defines if a system upgrade should be performed .|
| 'config_mgmt_interface_ipv4' | false | Specifies if a dedicated IPv4 management interface is used. |
| 'mgmt_interface_ipv4' | 0.0.0.0 | Defines IPv4 address of management interface. |
| 'config_mgmt_interface_ipv6' | false | Enable if (true) a dedicated IPv6 management interface is used. |
| 'mgmt_interface_ipv6' | :: | Defines IPv6 address for management interface. |
| 'config_ntp' | true | Defines if NTP should be configured. |
| 'ntp_servers' | ntp-pool-info_ntpp10.telekom.de, ntp-pool-info_ntpp21.telekom.de | Defines addresses of NTP servers to use. |
| 'set_timezone' | true | Defines if time zone should be configured. |
| 'use_timezone' | Europe/Berlin | Defines time zone to use. |
| 'config_syslog' | false |  Defines if Syslog should be configured. |
| 'syslog_type' | rsyslog | Defines type of syslog to use. |
| 'syslog_server' |  | Defines IP addresses of Syslog server(s) to use. |

Additional variables are located in the following files in directory '/vars':

* /vars
  * /main.yml
  * /RedHat.yml
  * /Ubuntu-18.yml
  * /Ubuntu.yml
  * /vars_linux(01)basic-hardening.yml
  * /vars_linux(02)logging.yml
  * /vars_linux(03)pam.yml
  * /vars_linux(04)iptables.yml
  * /vars_linux(05)mac.yml

-------------------------------------------------------------------------------

Note: Changing variables in these files can affect security compliance and must
      be approved by your responsible Project Security Manager!

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
* SecReq 3.65: Linux OS for Servers

**Not implemented requirements**

The following security requirements of the named document are not implemented in
this version of the Ansible role:

* Req 5:	Dedicated partitions must be used for growing content that can influence the availability of the system.

Note! Req 5 should be implemented during system installation!

* Req 6:	Parameters nodev, nosuid and noexec must be set for partitions where this is applicable.
* Req 20: GPG check for repository server must be activated and corresponding keys for trustable repositories must be configured.
* Req 21:	User accounts must be used that allow unambiguous identification of
the user.
* Req 24:	User accounts with extensive rights must be protected with two authentication attributes.
* Req 25:	The system must be connected to a central system for user administration.
* Req 27: The management of the operating system must be done via a dedicated management network which is independent from the production network.
* Req 29: Encrypted protocols must be used for management access to administrate
the operating system.
* Req 42:	If Syslog-NG is used, the default permission of 640 or more restrictive for logfiles must be configured.
* Req 43:	If Syslog-NG is used, at least one central logging server must be
configured.

The following requirements are not included as they are regular compliance checks and not relevant for hardening:

* Req 61:	No legacy + entries must exist in files passwd, shadows and group.
* Req 62	A user's home directory must be owned by the user and have mode 750 or more restrictive.
* Req 63:	Default group for the root account must be GID 0.
* Req 64:	Root must be the only UID 0 account.
* Req 65:	All groups in /etc/passwd must exist in /etc/group.
* Req 66:	No duplicate UIDs and GIDs must exist.
* Req 67:	No duplicate user and group names must exist.
* Req 68: The shadow group must be empty (only Ubuntu Linux).
* Req 69: No files and directories without assigned user or group must exist.
* Req 70:	Permissions of security relevant configuration files must have the distribution default values or more restrictive.

## License

Apache License, Version 2.0

See file LICENSE
