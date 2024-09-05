sap_aws_backint
===============

The role performs installation or upgrade of AWS Backint Agent as described in [the official documentation](https://docs.aws.amazon.com/sap/latest/sap-hana/aws-backint-agent-installing-configuring.html).
The stdout and stderr of the AWS Backint installer is saved in `/tmp/aws-backint-agent-install-output-<date>.log`. Debug log is saved in the home directory of the ansible user (`/home/sap-aws-ansible`).

*Important*
Any existing backup processes (including scheduled log backups) should be disabled before continuing with the installation/upgrade, if not an in-progress backup can be corrupted, which can impact ability to recover database.

## Role variables

- `sap_aws_backint_version`: see list of all the available versions in [the official documentation](https://docs.aws.amazon.com/sap/latest/sap-hana/aws-backint-agent-version-history.html).
- `sap_aws_backint_install_dir`, default is `/hana/shared`
- `sap_aws_backint_proxy`, default is `http://webproxy.prod.company.com:3128`

The following environment variables can be optionally defined on the Ansible control node:
- `SAP_AWS_BACKINT_VERSION` overrides `sap_aws_backint_version` variable.

## Dependencies

Role `aws_inventory` needs to be executed before this role.

## Example playbook

```yaml
---
- hosts: localhost
  gather_facts: False
  become: False
  roles:
    - aws_checks
    - role: aws_inventory
      vars:
        aws_inventory_filters_instance_states: running

- hosts: ec2hosts
  gather_facts: True
  become: True
  roles:
    - role: sap_aws_backint
      when:
        - ec2status.state.name == 'running'
        - ('Sap_env' in ec2status.tags)
        - ('Sap_deploy' in ec2status.tags)
        - ec2status.tags.Sap_deploy == 'True'
        - ('Sap_deploy_status' in ec2status.tags)
        - ec2status.tags.Sap_deploy_status == 'done'
        - ec2status.tags.Sap_class == 'db'

- hosts: localhost
  gather_facts: False
  become: False
  tasks:
    - name: Custom summary
      include_tasks: custom_summary.yml
```

## Testen van de backup

```
sudo -iu hdsadm
hdbsql -U HDB_SYSTEMDB "BACKUP DATA USING BACKINT ('MyBackup')"
hdbsql -U HDB_SYSTEMDB "BACKUP DATA FOR HDS USING BACKINT ('20240808')"
```

License
-------

MIT

Author Information
------------------

J. Hellemons
S. Lubbers
