hcloud_firewall
=========

Role to provision firewalls on Hetzner Cloud.

Requirements
------------

* Hetzner Python library on target host: [hcloud](https://pypi.org/project/hcloud/)
* Ansible Hetzner collection on control node: [Hetzner.Hcloud](https://docs.ansible.com/ansible/latest/collections/hetzner/hcloud/)

Role Variables
--------------

**Default Variables**

| Variable                     | Type | Description                                                        | Default                      |
|------------------------------|------|--------------------------------------------------------------------|------------------------------|
| hcloud_firewall_api_token_rw | str  | Read-write Hetzner Cloud api token                                 | -                            |

**Firewall defining variables**

These values are not predefined and need to be set as parameters when calling the role.


| Variable                  | Subitem | Type     | Description                              | Example        |
|---------------------------|---------|----------|------------------------------------------|----------------|
| hcloud_firewall_firewalls |         | lst/dict | List of firewalls to create              |                |
|                           | name    | str      | Name of firewall                         | my-firewall-01 |
|                           | state   | str      | State of firewall                        | present        |
|                           | rule    | lst/dict | List of firewall rules for each firewall |                |

**Firewall rule definitions**

| Variable    | Type     | Description                                     | Example          |
|-------------|----------|-------------------------------------------------|------------------|
| description | str      | Human readable description of rule              | "k8s api access" |
| direction   | str      | Traffic direction affected by rule (*in*/*out*) | in               |
| protocol    | str      | Network protocol (e.g. *tcp* or *icmp*)         | tcp              |
| port        | str      | Network port affected by rule                   | 443              |
| source      | list/str | List of source CIDRs allowed                    | - 0.0.0.0/0      |

Dependencies
------------

none

Example Playbook
----------------

    - name: "Manage firewalls on Hetzner Cloud."
      hosts: localhost
      gather_facts: false

      roles:

      - name: hcloud_firewall
        vars:
          hcloud_firewall_api_token_rw: "{{ vault_hcloud_token_rw }}"
          hcloud_firewall_firewalls:
            - name: my-firewall-01
              state: present
              rules:
                - description: ssh
                  direction: in
                  protocol: tcp
                  port: 22
                  source_ips:
                    - 0.0.0.0/0
                    - ::/0
                - description: icmp
                  direction: in
                  protocol: icmp
                  source_ips:
                    - 0.0.0.0/0
                    - ::/0
                - description: http
                  direction: in
                  protocol: tcp
                  port: 80
                  source_ips:
                    - 0.0.0.0/0
                    - ::/0
                - description: https
                  direction: in
                  protocol: tcp
                  port: 443
                  source_ips:
                    - 0.0.0.0/0
                    - ::/0

License
-------

BSD

Author Information
------------------

[flxsrbr](https://github.com/flxsrbr)
