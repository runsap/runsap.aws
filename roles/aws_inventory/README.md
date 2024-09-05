aws\_inventory
=============

This role creates a dynamic ansible inventory from the managed aws instances.

The dynamically added aws instances can be filtered. By default only running
 and managed instances are added.

Besides adding the instances to the ansible inventory a `host_data` dictionary
is added per host if the `aws` dict exists and the node is available in the
`group_vars` templated inventory, if the node is not available in this custom
inventory then the `host_data` dict will be empty.

The `host_data` dict will have the following structure
(for more info see terraform role):

```json
{ "host_data": {
      "ec2": {
          "availability_zone": "eu-central-1a",
          "domain": "sap.bk-eu-central2.dqs.company.com",
          "image_id": "ami-0fc46135fc3a40134",
          "tags": {
              "Ansible_managed": true,
              "Env": "DQS",
          },
      },
      "image": "CentOS-7.7",
      "mounts": {},
      "name": "sap-pipelinetest04a3",
      "place": "dr1",
      "template": "centos_v1"
    }
}
```


Requirements
------------

For access to AWS the appropriate environment variables must be defined.

Role Variables
--------------

The default filter for instances is defined in `aws_inventory_filters` variable. The default is:
```yaml
aws_inventory_filters:
  instance-state-name: running
  "tag:Managed": "yes"
```
For all possible filters see `ec2_instance_info` module.

All the instances are added to group defined in `aws_inventory_group` variable. The default group is `ec2hosts`.


Dependencies
------------

For populated `host_data` variables the `sap_inventory` role must be executed firstly
 (`check_mode` supported) and nodes need to be defined in the custom `group_vars`
 inventory accordingly.


Example Playbook
----------------

```yaml
- hosts: localhost
  gather_facts: False
  become: False
  roles:
    - aws_inventory

- hosts: ec2hosts
  gather_facts: True
  become: True
  roles:
    - aws_prepare
```

License
-------


Author Information
------------------

V. Elisseev
S. Lubbers

Data structure examples
-----------------------
### `efs` dictionary
```json
{
    "efs": {
        "autobank-bpi": {
            "encrypted": true,
            "region": [
                "pr"
            ]
        },
        "datasync-oip": {
            "encrypted": true,
            "region": [
                "pr",
                "dr"
            ]
        },
        "sapmnt-oip": {
            "encrypted": true,
            "region": [
                "pr"
            ]
        }
    }
```

### `aws_places` dictionary
```json
{
    "aws_places": {
        "dr1": {
            "availability_zone": "eu-central-1a",
            "domain": "presap.bk-eu-central2.dqs.company.com",
            "region": "eu-central-1",
            "security_group": "sg-0a86b532994e66488",
            "subnet_id": "subnet-09d9522406607e679",
            "subnets": [
                {
                    "assign_ipv6_address_on_creation": false,
                    "availability_zone": "eu-central-1a",
                    "availability_zone_id": "euc1-az2",
                    "available_ip_address_count": 248,
                    "cidr_block": "10.70.16.0/24",
                    "default_for_az": false,
                    "enable_dns64": false,
                    "id": "subnet-09d9522406607e679",
                    "ipv6_cidr_block_association_set": [
                        {
                            "association_id": "subnet-cidr-assoc-09e8bd62545ef5867",
                            "ipv6_cidr_block": "2a05:d014:cb7:e00f::/64",
                            "ipv6_cidr_block_state": {
                                "state": "associated"
                            }
                        }
                    ],
                    "ipv6_native": false,
                    "map_customer_owned_ip_on_launch": false,
                    "map_public_ip_on_launch": false,
                    "owner_id": "175912522491",
                    "private_dns_name_options_on_launch": {
                        "enable_resource_name_dns_a_record": false,
                        "enable_resource_name_dns_aaaa_record": false,
                        "hostname_type": "ip-name"
                    },
                    "state": "available",
                    "subnet_arn": "arn:aws:ec2:eu-central-1:175912522491:subnet/subnet-09d9522406607e679",
                    "subnet_id": "subnet-09d9522406607e679",
                    "tags": {},
                    "vpc_id": "vpc-0c8c189d071ef9bdf"
                }
            ],
            "vpc_id": "vpc-0c8c189d071ef9bdf"
        },
        ...
```
### `ec2status` dictionary
```json
{
    "ec2status": {
        "ami_launch_index": 0,
        "architecture": "x86_64",
        "block_device_mappings": [
            {
                "device_name": "/dev/sda1",
                "ebs": {
                    "attach_time": "2021-12-16T11:56:21+00:00",
                    "delete_on_termination": true,
                    "status": "attached",
                    "volume_id": "vol-0a1be19a418579625"
                }
            },
            {
                "device_name": "/dev/sdh",
                "ebs": {
                    "attach_time": "2021-12-16T11:56:43+00:00",
                    "delete_on_termination": false,
                    "status": "attached",
                    "volume_id": "vol-0ee87b49398f81eff"
                }
            },
            {
                "device_name": "/dev/sdg",
                "ebs": {
                    "attach_time": "2021-12-16T11:56:43+00:00",
                    "delete_on_termination": false,
                    "status": "attached",
                    "volume_id": "vol-02cf5edd611ac77cc"
                }
            }
        ],
        "capacity_reservation_specification": {
            "capacity_reservation_preference": "open"
        },
        "client_token": "443AA86F-7264-482B-91B0-3B36E92C35E7",
        "cpu_options": {
            "core_count": 1,
            "threads_per_core": 2
        },
        "ebs_optimized": true,
        "ena_support": true,
        "enclave_options": {
            "enabled": false
        },
        "hibernation_options": {
            "configured": false
        },
        "hypervisor": "xen",
        "iam_instance_profile": {
            "arn": "arn:aws:iam::271601252699:instance-profile/BookingInstanceProfile",
            "id": "AIPAT6PFSHFN2Y63H23OC"
        },
        "image_id": "ami-0f0692db9802d4927",
        "instance_id": "i-08a6f283ff179a9bf",
        "instance_type": "t3.small",
        "key_name": "sapinstall",
        "launch_time": "2021-12-16T11:56:21+00:00",
        "metadata_options": {
            "http_endpoint": "enabled",
            "http_protocol_ipv6": "disabled",
            "http_put_response_hop_limit": 1,
            "http_tokens": "optional",
            "state": "applied"
        },
        "monitoring": {
            "state": "disabled"
        },
        "network_interfaces": [
            {
                "attachment": {
                    "attach_time": "2021-12-16T11:56:21+00:00",
                    "attachment_id": "eni-attach-014bbf4b8059b7fdd",
                    "delete_on_termination": false,
                    "device_index": 0,
                    "network_card_index": 0,
                    "status": "attached"
                },
                "description": "",
                "groups": [
                    {
                        "group_id": "sg-049b7b2708911d19b",
                        "group_name": "open_for_all"
                    }
                ],
                "interface_type": "interface",
                "ipv6_addresses": [],
                "mac_address": "02:8a:5d:97:06:cb",
                "network_interface_id": "eni-0d015838842ad0bca",
                "owner_id": "271601252699",
                "private_dns_name": "ip-10-96-0-152.eu-west-1.compute.internal",
                "private_ip_address": "10.96.0.152",
                "private_ip_addresses": [
                    {
                        "primary": true,
                        "private_dns_name": "ip-10-96-0-152.eu-west-1.compute.internal",
                        "private_ip_address": "10.96.0.152"
                    }
                ],
                "source_dest_check": true,
                "status": "in-use",
                "subnet_id": "subnet-0117ab3cbaf3e8c40",
                "vpc_id": "vpc-0c16a7f9984bf634d"
            }
        ],
        "placement": {
            "availability_zone": "eu-west-1a",
            "group_name": "",
            "tenancy": "default"
        },
        "platform_details": "Red Hat Enterprise Linux",
        "private_dns_name": "ip-10-96-0-152.eu-west-1.compute.internal",
        "private_dns_name_options": {
            "enable_resource_name_dns_a_record": false,
            "enable_resource_name_dns_aaaa_record": false,
            "hostname_type": "ip-name"
        },
        "private_ip_address": "10.96.0.152",
        "product_codes": [
            {
                "product_code_id": "cpu6418va7cq42340jun0midl",
                "product_code_type": "marketplace"
            }
        ],
        "public_dns_name": "",
        "root_device_name": "/dev/sda1",
        "root_device_type": "ebs",
        "security_groups": [
            {
                "group_id": "sg-049b7b2708911d19b",
                "group_name": "open_for_all"
            }
        ],
        "source_dest_check": true,
        "state": {
            "code": 16,
            "name": "running"
        },
        "state_transition_reason": "",
        "subnet_id": "subnet-0117ab3cbaf3e8c40",
        "tags": {
            "Ansible_managed": "True",
            "Env": "dev",
            "Fqdn": "sap-ztscs18a1.presap.bk-eu-west7.dqs.company.com",
            "Gid": "zts",
            "Name": "sap-ztscs18a1",
            "Patch Group": "dev",
            "Patch_group": "sap-vrhel-test",
            "Project": "pipeline",
            "Provision": "puppet",
            "Provision_status": "done",
            "Provision_storage": "True",
            "Releasever": "7Server",
            "Resource_type": "ec2",
            "Sap_class": "ascs",
            "Sap_deploy": "True",
            "Sap_env": "ZTS",
            "Sap_role": "prim",
            "Sap_sid": "ZTS",
            "Sap_type": "abap",
            "Sap_version": "NW752",
            "Sap_virtual_ascs_name": "zts-cs.sap.company.com",
            "Sox": "False"
        },
        "usage_operation": "RunInstances:0010",
        "usage_operation_update_time": "2021-12-16T11:56:21+00:00",
        "virtualization_type": "hvm",
        "vpc_id": "vpc-0c16a7f9984bf634d"
    }
}
```
