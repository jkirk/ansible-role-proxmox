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

None.

Additional Note
---------------

If you use the role [Oefenweb/ansible-hostname](https://github.com/Oefenweb/ansible-hostname) to set the hostname and `/etc/hosts` you should put the following in `group_vars/proxmox`:

```yaml
---
hostname_hostname_ip_address: "{{ ansible_default_ipv4.address }}"
```

to avoid changes `/etc/hosts` by this role.

That is because `Oefenweb/ansible-hostname` makes sure that the following line exists in `/etc/hosts`:

```
127.0.1.1 proxmox.example.com proxmox
```

but Proxmox **needs** the external IP-Addresss to be resolvable. That means the `/etc/hosts` line should look like this:

```
192.0.2.1 proxmox.example.com proxmox
```

Example Playbook
----------------

For this example we will assume you have defined a host group *proxmox* in the inventory file `hosts`.

`site-proxmox.yml`:

```
---

    - hosts: proxmox
      roles:
         - role: proxmox
```

To run this playbook you would do `ansible-playbook -i hosts site-proxmox.yml`

License
-------

MIT

Author Information
------------------

Darshaka Pathirana - https://synpro.solutions
