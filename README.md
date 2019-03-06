# bigswitch

This Ansible Network role provides a set of platform dependent functions that are designed to work with Big Switch Cloud Fabric. The functions will include both configuration and fact collection.

## Requirements

* Ansible 2.6 or later
* Ansible [network-engine](https://galaxy.ansible.com/ansible-network/network-engine)

## Functions

This section provides a list of the available functions that are including in this role. Any of the provided functions can be implemented in Ansible playbooks to perform automation activities on Big Switch Cloud Fabrics.

* get_facts [[source]](/tasks/get_facts.yml)

## Example

Below is an example playbook used to gather facts from a Big Switch fabric. The default connection variables are below. Override these variables in your inventory or playbook.

### Connection Variables

```yaml
bsn_controller_addr: 127.0.0.1
bsn_controller_port: 8443
bsn_username: admin
bsn_password: bsn123
bsn_validate_certs: no
```

### Playbook

```yaml
---
- hosts: localhost
  connection: local
  gather_facts: no
  vars:
    - bsn_controller_addr: "{{ ansible_host }}"
    - ansible_network_os: bsn

  roles:
    - ansible-network.network-engine

  tasks:
  - include_role:
      name: bigswitch
      tasks_from: get_facts
    vars:
      subset:
        - default

  - debug:
      var: bigswitch
```

### Output

```json
TASK [debug] 
*******************************
ok: [localhost] => {
    "bigswitch": {
        "logging": {
            "servers": [
                {
                    "address": "1.1.1.1", 
                    "log_level": null, 
                    "port": "514"
                }, 
                {
                    "address": "2.2.2.2", 
                    "log_level": null, 
                    "port": "514"
                }
            ]
        }, 
        "system": {
            "build_date": "2018-05-24T05:10:52.000Z", 
            "build_user": "bsn", 
            "ci_build_bumber": "186", 
            "ci_job_name": "bcf/bcf-4.7.0", 
            "community_edition": false, 
            "edition": "standard", 
            "product_type": "BCF", 
            "release_string": "Big Cloud Fabric 4.7.0 (bcf-4.7.0 #186)", 
            "version": {
                "maint": "0", 
                "major": "4", 
                "minor": "7", 
                "string": "4.7.0"
            }
        }
    }
}
```