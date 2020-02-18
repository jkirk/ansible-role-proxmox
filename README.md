proxmox
=======

Simple ansible role to install Proxmox VE on Debian as described in https://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_Stretch and https://pve.proxmox.com/wiki/Install_Proxmox_VE_on_Debian_Buster

Requirements
------------

None.

Role Variables
--------------


If the role variable `proxmox_pveadmins` is defined a PVE group 'admin' is
created and the ACL is updated in that way that members of the group 'admin'
are assigned the 'Administrator' role for the whole system (path `/`):

```
% sudo pvesh get /access/acl
┌──────┬───────────────┬───────┬───────┬───────────┐
│ path │ roleid        │ type  │ ugid  │ propagate │
╞══════╪═══════════════╪═══════╪═══════╪═══════════╡
│ /    │ Administrator │ group │ admin │ 1         │
└──────┴───────────────┴───────┴───────┴───────────┘
```

The list of users which will be added to this admin group is set with this role variable:

```
# proxmox_pveadmins: [ 'janedoe' , 'johndoe' ]
```

Dependencies
------------

None.

Additional Notes
----------------

If you use the role [Oefenweb/ansible-hostname](https://github.com/Oefenweb/ansible-hostname) (which sets the hostname and `/etc/hosts`)  you should put the following in `group_vars/proxmox`:

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
         - { role: proxmox, proxmox_pveadmins: [ 'janedoe', 'johndoe' ] }
```

To run this playbook you would do `ansible-playbook -i hosts site-proxmox.yml`

License
-------

MIT

Author Information
------------------

Darshaka Pathirana - https://synpro.solutions
