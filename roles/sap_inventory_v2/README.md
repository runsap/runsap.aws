sap_inventory
=============

Used for processing the custom group_vars templated inventory and generating
 the `aws` variable which can be used in other roles. Below is the structure
 of this variable for one of the instances:
```yaml
aws:
  hosts:
    - ec2:
        availability_zone: eu-central-1a
        domain: sap.bk-eu-central2.dqs.company.com
        ebs_optimized: true
        image_id: ami-00e9db6661b279d3f
        instance_type: m5.large
        key_name: sapinstall
        region: eu-central-1
        security_group: sg-0214bdf39487e5446
        subnet_id: subnet-0509cea31c2f00fd3
        subnets:
        - assign_ipv6_address_on_creation: false
          availability_zone: eu-central-1a
          availability_zone_id: euc1-az2
          available_ip_address_count: 247
          cidr_block: 10.70.19.0/24
          default_for_az: false
          id: subnet-0509cea31c2f00fd3
          ipv6_cidr_block_association_set:
          - association_id: subnet-cidr-assoc-0b6de88e6f1271a86
            ipv6_cidr_block: 2a05:d014:cb7:e012::/64
            ipv6_cidr_block_state:
              state: associated
          map_customer_owned_ip_on_launch: false
          map_public_ip_on_launch: false
          owner_id: '175912522491'
          state: available
          subnet_arn: arn:aws:ec2:eu-central-1:175912522491:subnet/subnet-0509cea31c2f00fd3
          subnet_id: subnet-0509cea31c2f00fd3
          tags: {}
          vpc_id: vpc-0c8c189d071ef9bdf
        tags:
          Ansible_managed: 'True'
          Env: dqs
          Project: sandbox
          Provision: puppet
          Provision_storage: 'True'
          Sap_class: ascs
          Sap_type: abap
          Sox: 'False'
        volumes:
        - device_name: /dev/sda1
          ebs:
            encrypted: true
            volume_size: 30
            volume_type: gp2
        vpc_id: vpc-0c8c189d071ef9bdf
        vpc_subnet_id: subnet-0509cea31c2f00fd3
      efs: {}
      gid: codi-fe
      id: '01'
      image: RHEL-8.2.0-SAP-HVM-20200423
      mounts:
        /usr/sap:
          backup_policy_ebs: daily_14
          fstype: xfs
          lvol: local
          size: 50
          type: gp2
          vg: sap
      name: sap-codi-fe01a3
      place: dr1
      template: sap_ascs_v1
```

Requirements
------------

 The role `aws_inventory` should executed before using this role.

Role usage
-----------

```yaml
- hosts: localhost
  gather_facts: False
  become: False
  roles:
    - sap_inventory
```

Dependencies
------------

Example Playbook
----------------

License
-------

Author Information
------------------

V. Elisseev
S. Lubbers