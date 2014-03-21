authoritative_dns
=================

This roles enables users to configure the dns servers 

- BIND9
- NSD3

As the BIND9 configuration could be complex, this role is recommend to use
only for autoritative DNS setup as the NSD is not able to handle recursive queries.

BIND9 and NSD3 was choosen for their widely usage in ROOT and TLD zones.
You're welcome do add any other modules for another dns server implementation.

Requirements
------------
This role requires Ansible 1.4 or higher, and platform requirements are listed
in the metadata file. It should work also with lower version, but I have never tested it.

Role Variables
--------------
Please readme the descriptions in the

  - defautls/main.yml
  - vars/main.yml

The variables in the `defautls/main.yml` could be overriden in the `host_vars`/`group_vars`.
The variables in the `vars/main.yml` are global for all managed servers.

### /etc/ansible/hosts

    [dns-bind9]
    host1
    host2

    [dns-nsd3]
    host3
    host4

    [dns:children]
    dns-bind9
    dns-nsd3

The group names `dns-bind9` and `dns-nsd3` are mandaroty. The tasks depenends on these names.

Dependencies
------------

None

License
-------

BSD

Author Information
------------------

Peter Hudec
