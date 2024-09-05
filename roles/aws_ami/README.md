aws_ami
========

This role used to find aws ami id's on per region basis using filters
 availabvle in the `aws_image` dictionary. If more
 then one image matching the defined search filter found then the newest is taken.
 For complete list of filters see [AWS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeImages.html).
 This dctionaly will be extended dynamically with ami id's on per region basis.
 Assume that the dictionary is definad as on the example below:
```yaml
aws_image:
  "CentOS-7":
    filters:
      name: "{{ 'sap_base' if aws_env == 'prod' else 'sap_base_dev' }}-*"
      "tag:distribution": CentOS
      "tag:distribution_major_version": 7
    user: sap-aws-ansible
  "SAP-7.5_HVM_GA":
    ...
```
This role will dynnamicarry extend this dictionary with the image ID's accordingly:
```yaml
aws_image:
  "CentOS-7":
    filters:
      name: "{{ 'sap_base' if aws_env == 'prod' else 'sap_base_dev' }}-*"
      "tag:distribution": CentOS
      "tag:distribution_major_version": 7
    eu-central-1: ami-0719fa473c59f3ee1
    eu-west-1: ami-0a4ee0399c7705417
    user: sap-aws-ansible
  "SAP-7.5_HVM_GA":
    ...
```
For easier ures is to ami image mapping the following dictionary additionally created:
```yaml
aws_image_users:
  ami-0719fa473c59f3ee1: sap-aws-ansible
  ami-0a4ee0399c7705417: sap-aws-ansible
```


Requirements
------------

For access to AWS the appropriate environment variables must be defined.


Role Variables
--------------

Dependencies
------------

Example Playbook
----------------

```yaml
- hosts: localhost
  gather_facts: False
  become: False
  roles:
    - aws_ami
```

License
-------
MIT

Author Information
------------------

V. Elisseev
S. Lubbers