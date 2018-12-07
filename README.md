proxmox
=======

Simple ansible role to install Proxmox VE on Debian as described in https://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_Stretch

Requirements
------------

None.

Role Variables
--------------

None.

Dependencies
------------

Loosly depends on role `jkirk/ansible-role-base` as it assumes that *etckeeper* is installed.

Example Playbook
----------------

For this example we will assume you have defined a host group *proxmox* in the inventory file `hosts`.

`site-proxmox.yml`:

```
---

    - hosts: proxmox
      roles:
         - { role: proxmox }
```

To run this playbook you would do `ansible-playbook -i hosts site.yml`

License
-------

MIT

Author Information
------------------

Darshaka Pathirana - https://synpro.solutions
