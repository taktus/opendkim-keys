Role Name
=========

Configure dkim keys and KeyTable and SingingKeys (opendkim)a for give list of domains
If enabled DNS TXT records can be added to Cloudflare DNS service

Requirements
------------

The role is not designed to configure opendkim packet, it assumes opendkim is installed and configured
and opendkim daemon works properly 


Role Variables
--------------

opendkim_conf_folder - the path to opendkim config path (absolute) - default /etc/opendkim
opendkim_keys_folder - the path to keys folder (absolute)          - default /etc/openkim/keys 
opendkim_domains:    - the dictionary containing domains           - default []

the dictionary of the domains should look like:

```
opendkim_domains:
   - name: 
     selector: 
     state: 
     bits:
```

where:

opendkim_domain_selector - string - default 'default'
opendkim_domain_state    - (present/apsent) -  default present
opendkim_key_bits        - number of bits used to generate keys        - default 2048


Cloudflare integration 

The role is able to update Cloudflare DNS TXT records automatially exporting the value of public keys generated
To enable CF integration functionality the following variables has to be defined:

opendkim_cf  - must be equal to yes
opendkim_cf_mail - has to be set to email address used by CF API
opendkim_cf_api_token - has to be set to global api token  used by CF API

for detaiuls see https://docs.ansible.com/ansible/latest/modules/cloudflare_dns_module.html


Dependencies
------------

- none

Example Playbook
----------------

Basic usage
```
    - hosts: mailserver
      roles:
         - role: taktus/opendkim-keys
           vars:
             - opendkin_cf: no
             - opendkim_domains:
               - name: example.com
                 selector: default
                 state: present
               - name: foo.bar
                 selector: mail
                 state: absent
```

Cloudflare integration
```
    - hosts: mailserver
      roles:
         - role: taktus/opendkim-keys
           vars:
             - opendkin_cf: yes
             - opendkim_cf_api_token: 1234567890abcdef
             - opendkim_cf_mail: cfmaster@example.com
             - opendkim_domains:
               - name: example.com
                 selector: default
                 state: present
               - name: foo.bar
                 selector: mail
                 state: absent
```

License
-------

BSD

Author Information
------------------

MarQ (lan projekt)  https://lp.net.pl
