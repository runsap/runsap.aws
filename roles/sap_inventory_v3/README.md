runsap.aws.sap_aws_inventory
============================

[![Lint Ansible Collection](https://github.com/runsap/runsap.aws/actions/workflows/lint.yml/badge.svg)](https://github.com/runsap/runsap.aws/actions/workflows/lint.yml)

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

Migrating from sap_inventory and aws_inventory
----------------------------------------------

The following changes are taken from the original pull request: https://gitea.tooling.automatesap.com/CCS/sap-aws-1/pulls/453/files

- Add subnets to aws places

- Change template for sap_hosts in `landscape.tfvars.j2` as follows:
  ```
  sap_hosts = {
          name = "{{ host_name }}"
          instance_type = "{{ host_data.ec2.instance_type }}"
          availability_zone = "{{ host_data.ec2.availability_zone }}"
          # remove ami and add ami_filter_name
          ami_filter_name = "{{ aws_image[host_data.ec2.image].filters.name }}" # Retrieve the AMI filter
          iam_instance_profile = "{{ host_data.ec2.iam_instance_profile }}"
          security_groups = {{ host_data.ec2.security_groups | to_json }}
          sap_domain = "{{ sap_domain }}"
  ```

- Change roles to version 3
  ```
      # - name: runsap.aws.aws_ami
      # - name: runsap.aws.sap_inventory
      # - name: runsap.aws.aws_inventory
      - name: runsap.aws.sap_inventory_v3
  ```

- Change `terraform/modules/aws_ec2/main.tf`
  Add
  ```
  data "aws_ami" "lookup" {
    most_recent = true

    filter {
      name   = "name"
      values = [ var.ami_filter_name ]
    }
  }
  ```
    and change the ami line into:
  ```
  ami = data.aws_ami.lookup.id
  ```

- Change `terraform/modules/aws_ec2/vars.tf`
  ```
  # variable "ami" {
  variable "ami_filter_name" {
    type        = string
    type        = string
  }
  ```

- Change all ami values into ami_filter_name `terraform/sap_hosts.tf`

