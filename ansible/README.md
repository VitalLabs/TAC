Dependencies for AWS use:

You must have AWS CLI installed (NOT the older EC2 CLI) and configured.  We use shell commands to augment where core EC2 modules don't support actions.

You must be running the patched ec2_vps module, and using devel branch as of Feb 27.  Until this module is released with 1.5, here are those instructions:

git clone https://github.com/risaacson/ansible.git
cd ansible
git checkout dfed_vpc
source /hacking/env-setup

Then cd into this playbook directory and run as needed.


Note: For a environment deployment you must have the following file  on your local hard drive to configure secure settings:

switchboard_config: /.ssh/switchboard_vars.yml