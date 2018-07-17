Ansible Role for RADIUS
=========

Configures [FreeRADIUS](https://freeradius.org) server and PPP client

Requirements
------------

None.

Role Variables
--------------

```YAML
radius_packages:
 - freeradius
radius_config_base: /etc
radius_config_dir: "{{ radius_config_base }}/freeradius/3.0"
radius_client_dir: "{{ radius_config_base }}/radiusclient"
radius_service_name: freeradius
radius_user_name: freerad
radius_config:
  is_server: true
  users: []
  server:
    address: 127.0.0.1
  clients: []
```

Dependencies
------------

None.

Example Playbook
----------------

Install client configuration files to be used by a PPP daemon with the
corresponding RADIUS server on the same machine.
```YAML
- hosts: vpn_server
  become: yes
  become_user: root
  roles:
    - role: radius
      vars:
        radius_config:
          is_server: false
          server:
            address: 127.0.0.1
            secret: secretxyz
      tags: [ radius, client ]

- hosts: radius_server
  become: yes
  become_user: root
  roles:
    - role: radius
      vars:
        radius_config:
          is_server: true
          address: "{{ ansible_default_ipv4.address }}"
          clients:
            - name: localhost
              address: 127.0.0.1
              secret: secretxyz
          users:
            - name: test
              password: 123
      tags: [ radius, server ]
```

License
-------

MIT
