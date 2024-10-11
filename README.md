Ansible Collection - runsap.aws
===============================

![Ansible Lint](https://github.com/runsap/runsap.aws/actions/workflows/lint.yml/badge.svg)

Runsap Introduction
-------------------
Runsap is a method of installing and managing large or small SAP focussed landschapes. This collection is part of, and should be used with, the other runsap collections for optimal usage.

Collection introduction
-------------------------
This collection helps you in managing several aws activities.

**Roles**

- aws_ami (deprecated)
- aws_backint
- aws_intentory (deprecated)
- sap_inventory (deprecated)
- sap_inventory_v3

**_v3**
From the roles the v3 role is the most recent role. It's a rewrite of a combination of the other roles. The AWS AMI is no longer looked up in the ansible code.
