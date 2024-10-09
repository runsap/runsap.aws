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

- aws_ami
- aws_backint
- aws_intentory
- sap_inventory

**aws_ami**
The **aws_ami** role is used to collect the ami id to present this to the terraform role so it can be input for the terrform run.