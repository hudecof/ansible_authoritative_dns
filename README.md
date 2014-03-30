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

To install `nsd3` on RedHat/CentOS you need to use EPEL repository. Upon request I can add tasks to the RedHat setup section to add EPEL repository.

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

The group names `dns-bind9` and `dns-nsd3` are mandatory. The tasks depenends on these names.

### vars/main.yml

    dns_zone_data:
      example.com:
        zone: example.com
        file: db.com.example
        descr: "Example zone configuration"

The `dns_zone_data` disctionary holds all zonem names where
- `dictionary key` is just key. You can choose any value as you want. I use the zone name
- `zone` is the name if the zone
- `file` is the name if the filename, where are the zone data stored. I user reverse order of the tokens prefixed by db.
- `descr` is usefull description. It will be as comment in configuration file

### defauls/main.yml
Among all variables the most important are

    dns_keys_tsig: []

list of tsig configuration files
there will used template file

- templates/bind9/bind_cfg_key.{{ item }}.j2 for bind9
- templates/nsd3/nsd_cfg_key.{{ item }}.j2 for nsd3

    dns_zones:
      - zone: example.com
        template: slave-ns-nokey

list of the zones to server
the 'zone' item is key into 'dns_zone_data' structure in vars/main.yml
the 'templates' item is a jinja2 template file in templates/(bind|nsd)

- templates/bind9/bind_cfg_zone.{{ item }}.j2 for bind9
- templates/nsd3/nsd_cfg_zone.{{ item }}.j2 for nsd3

Dependencies
------------

None

License
-------

BSD

Author Information
------------------

Peter Hudec
