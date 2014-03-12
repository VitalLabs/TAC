This directory contains Packer templates for the various AMIs. 

Install Packer first: http://packer.io

You can build a new AMI by running, for example:

```
packer build cassandra.packer
```

In general, the Packer playbooks will need to call the same roles, minus
any configuration or live system setup tasks. The roles can be instrumented
by adding when: ```packer_build is not defined and not packer_build``` to
the task in question, and during a Packer build, this task will not run.
