Dependencies for AWS use:

You MUST use a stable version of ansible. 1.5.0 for right now.

pip install ansible==1.5.0

You must have AWS CLI installed (NOT the older EC2 CLI) and configured.  We use shell commands to augment where core EC2 modules don't support actions.

You must be running the patched ec2_vps module, and using devel branch as of Feb 27.  Until this module is released with 1.5, here are those instructions:

git clone https://github.com/risaacson/ansible.git
cd ansible
git checkout dfed_vpc
source /hacking/env-setup

Then cd into this playbook directory and run as needed.

Proper command to run playbooks example:

ansible-playbook -vvvv -i /Users/ryanwmedlin/projects/switchboard-infrastructure/ansible/inv/ 1-dev-infrastructure.yml


Note: For a environment deployment you must have the following file  on your local hard drive to configure secure settings:

switchboard_config: /.ssh/switchboard_vars.yml

Example settings for this file:

 twilio_sid: "****"
 twilio_token: "****"


 service_smtp_user: "********************************"
 service_smtp_pass: "********************************"



 If you run into any Control Path erorrs then set this environment variable:

 export  ANSIBLE_SSH_CONTROL_PATH="%(directory)s/%%h-%%r"





 S3 Setup:

 You do need ot place github keys in your AWS accoutns S3 bucket if your using a different account:

 /vr-chef-dev/deploy*.pub


 JumpHost Key setup:

 You will need to export your 
 AWS access keys AND ssh-agent/ ssh-add your key that ansible is configure to use.  Keeping this step manual for now since keys should be local and secure.

  Fast Testing using Jumphost:

 you can ssh to the jumphost to run these scripts. Since they are on the same betwork they will run alot faster.

 /var/lib/swb-ansible contrains the latest deployment scripts.




