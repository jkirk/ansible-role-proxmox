proxmox
=======

Simple ansible role to install Proxmox VE on Debian as described in <https://pve.proxmox.com/wiki/Category:Installation>

Requirements
------------

None.

Role Variables
--------------

If the role variable `proxmox_pveadmins` is defined a PVE group 'admin' is
created and the ACL is updated in that way that members of the group 'admin'
are assigned the 'Administrator' role for the whole system (path `/`):

```sh
% sudo pvesh get /access/acl
┌──────┬───────────────┬───────┬───────┬───────────┐
│ path │ roleid        │ type  │ ugid  │ propagate │
╞══════╪═══════════════╪═══════╪═══════╪═══════════╡
│ /    │ Administrator │ group │ admin │ 1         │
└──────┴───────────────┴───────┴───────┴───────────┘
```

The list of users which will be added to this admin group is set with this role variable:

```yaml
# proxmox_pveadmins: [ 'janedoe' , 'johndoe' ]
```

Dependencies
------------

None.

Additional Notes
----------------

If you use the role [Oefenweb/ansible-hostname](https://github.com/Oefenweb/ansible-hostname) (which sets the hostname and `/etc/hosts`)  you should put the following in `group_vars/proxmox.yml` (together with a host group `proxmox`):

```yaml
---
hostname_hostname_ip_address: "{{ ansible_default_ipv4.address }}"
```

to avoid changes `/etc/hosts` by this role.

That is because `Oefenweb/ansible-hostname` makes sure that the following line exists in `/etc/hosts`:

```lang-txt
127.0.1.1 proxmox.example.com proxmox
```

but Proxmox **needs** the external IP-Addresss to be resolvable. That means the `/etc/hosts` line should look like this:

```lang-txt
192.0.2.1 proxmox.example.com proxmox
```

Further more, if you have use a Proxmox/Ceph cluster with two rings and a ceph network, a host group `cluster` is needed and something like this should be added in `group_vars/cluster.yml`:
```yaml
---
hostname_additional_hosts:
  - ip_address: 10.88.0.1
    hostname: alpha01.ring0
  - ip_address: 10.88.0.2
    hostname: alpha02.ring0
  - ip_address: 10.88.0.3
    hostname: alpha03.ring0
  - ip_address: 10.89.0.1
    hostname: alpha01.ring1
  - ip_address: 10.89.0.2
    hostname: alpha02.ring1
  - ip_address: 10.89.0.3
    hostname: alpha03.ring1
```

Example Playbook
----------------

For this example we will assume you have defined a host group *proxmox* in the inventory file `hosts`.

`site-proxmox.yml`:

```yaml
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

Darshaka Pathirana - <https://synpro.solutions>
