runsap.aws.sap_aws_inventory
============================

Introduction
------------

This role creates a dynamic inventory of all the hosts that match the filter.

The sap_groups, host_defaults and the templates are merged together in this role and the data from these merges are added to the hosts in the form of the host_data variable.

**Workflow**
1. Get all hosts that match the aws filter
2. Create the host_data variable
3. Add hosts to dynamic inventory with the host_data

Example
-------

```yaml
- name: Create the ec2host inventory with running instances
  hosts: localhost
  gather_facts: False
  become: False
  roles:
    - role: runsap.aws.sap_inventory_v3
      vars:
        aws_inventory_filters:
          instance-state-name: ["running"]
```