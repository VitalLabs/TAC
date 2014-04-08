##Dependencies for AWS use:


Export your AWS credentials as usual:

These are usually all the same in staging/prod but dev might be different sometimes when doing ansible development.

   export AWS_ACCESS_KEY_ID="****"
   export AWS_SECRET_ACCESS_KEY="******"
   
   #location of archive builds in s3
   export AWS_ACCESS_KEY_ID_VR="****"
   export AWS_SECRET_ACCESS_KEY_VR="******"

   #location of route53 domain names..  needed if in a dev AWS account when making big changes!!
   export AWS_ACCESS_KEY_ID_VR="****"
   export AWS_SECRET_ACCESS_KEY_VR="******"

   ssh-agent bash
   ssh-add ~/.ssh/keypair.pem


You MUST use a stable version of ansible. 1.5.0 for right now.

pip install ansible==1.5.0

You must have AWS CLI installed (NOT the older EC2 CLI) and configured.  We use shell commands to augment where core EC2 modules don't support actions.

If you will be using dedicated VPC instances then we will need to use a patched module. We will ping ansible when that time comes. This is WIP

Then cd into this ./ansible playbook directory and run as needed.


## Quickstart for Production



Deploy all code to orchestra and switchboard by logging into jump box and running:

ansible-playbook -vvvv -i ./inv update-all-application-code.yml -e environ=production


See ansible/README.md file of rather detailed info:

Repl:

ubuntu@ec2-54-82-72-21.compute-1.amazonaws.com -L 4055:127.0.0.1:4005 -N


Jumpbox access:

Export your keys and add ssh keys

bin/login-ansible tag_Name_production_jumphost

or 

ssh -i ~/.ssh/swb-production-keypair.pem ~/.ssh/orchestra.md.key ubuntu@ec2-54-226-179-206.compute-1.amazonaws.com

Ansible bugs:

If any bugs in ansible (I hope not!!!) I usually will make changes to my local master or a branch and rsync up to the jump box.

Example:

rsync -avze ssh -i ~/.ssh/swb-production-keypair.pem --delete /Users/ryanwmedlin/projects/switchboard-infrastructure/ansible/ ubuntu@ec2-54-226-179-206.compute-1.amazonaws.com:/var/lib/sw_ansible/ansible


##Proper commands to run playbooks:

First Provision EC2 instances

'''
ansible-playbook -vvvv -i ./inv/ 1-staging-infrastructure.yml -e environ=staging
'''

Then install all core software and apply configurations. Right now both are together. We can break this out into a AMI image build in the future.

bootstrap the jumphost to run the deployments:

'''
ansible-playbook -vvvv -i ./inv/ bootstrap-jumphost.yml -e environ=staging
'''

You can then log into the jumphost abnd run tmux new or tmux attach to run commands there ALOT faster.

'''
bin/login-ansible tag_Name_staging_jumphost
'''

Install everything:

'''
ansible-playbook -vvvv -i ./inv/ 2-install-and-configs.yml -e environ=staging
'''

##Environment Consideration

To create a new environment then create a corresponding config and setup playbook and pass in -e environ=staging to the ansible-playbooks command.  Otherwise it will always default to dev.


ansible-playbook -vvvv -i ./inv/ 2-install-and-configs.yml -e environ=staging


##Ad Hoc Playbooks:

Example: ansible-playbook -vvvv -i ./inv/ depploy-app-deploy.yml


Command to deploy new code:
ansible-playbook -vvvv -i ./inv update-all-application-code.yml -e environ=staging


Immutant is the biggest pain right now, her eis the command to restart it:

asnsible-playbook -vvvv -i ./inv update-immutant-configs.yml -e environ=staging



deploy-app-deploy.yml:  This installs the Switchboard app via the app-deploy role and restarts the server.

deploy-app-clojure-config-source.yml:  This installs the Orchestra app via the appropriate role and restarts the server.


update-cassandra.yml:  Updates cassandra config and does a rolling update. No server downtime expected.

update-*  bare bones placeholder for more advanced rolling updates.  Right now we are relying on the nginx failover facilities for this so we ight not need these.



Note: For a environment deployment you must have the following file  on your local or jumpbox machine to configure secure settings:

switchboard_config: ~/.ssh/switchboard_vars.yml

Example settings for this file:

 twilio_sid: "****"
 twilio_token: "****"


 service_smtp_user: "********************************"
 service_smtp_pass: "********************************"



 If you run into any Control Path erorrs or random SSH errors then set this environment variable:

 export  ANSIBLE_SSH_CONTROL_PATH="%(directory)s/%%h-%%r"

 If you run into any ssh key errors its best to just edit your ~/.ssh/config file and add the following line:

 Host *.amazonaws.com
    ControlPath ~/.ssh/co-%h
    IdentityFile ~/.ssh/keyfile.pem




## S3 Setup:

 You do need to place github keys in your AWS accounts S3 bucket if your using a different account:

 /vr-chef-dev/deploy*.pub

 These have already been added to the VR account.

## Utilities

 bin folder has useful utilities for setting up your connections

 ## Utilities-playbooks

 this is where we will keep adding one off commands as DevOps workflows evolve:

 update-all-applications-code:  You can pass in a verion to pull and deploy for both orchestra and switchboard.


 Handy Command to get hosts info:  ansible -i ./inv "tag_Name_dev_app" --list-hosts


## JumpHost Key setup:

 You will need to export your 
 AWS access keys AND ssh-agent/ ssh-add your key that ansible is configure to use.  Keeping this step manual for now since keys should be local and secure.

 Fast Testing using Jumphost:

 you can ssh to the jumphost to run these scripts. Since they are on the same betwork they will run alot faster.

 /var/lib/swb-ansible contains the latest deployment scripts.

 ## Test environment and Inventory Files

 I also use my AWS account for initial testing to reduce risk.

 you must set these properties in the group_vars/all  in order to do this.

 #Setting to test East
ec2_region: us-east-1
ec2_zone: us-east-1a
ec2_image: ami-9c78c0f5

ec2_keypair: ryanvitalreactor-west

ec2_identifier: vr-primary-acct

Note that when switching regios you also must update the ./inv/ec2.ini fiole to use the current region.

### SSH and SSL Keys

Note these keys are local to you and are kept in a secure location and uploaded to a new jumpbox and kept in the .ssh folder so no one else can access them!


➜  ansible git:(master) ✗ scp -i ~/.ssh/swb-production-keypair.pem ~/.ssh/switchboard_vars.yml ubuntu@ec2-54-226-179-206.compute-1.amazonaws.com:~/.ssh
switchboard_vars.yml                                                                                                       100%  767     0.8KB/s   00:00
➜  ansible git:(master) ✗ scp -i ~/.ssh/swb-production-keypair.pem ~/.ssh/swb-production-keypair.pem ubuntu@ec2-54-226-179-206.compute-1.amazonaws.com:~/.ssh
swb-production-keypair.pem                                                                                                 100% 1692     1.7KB/s   00:00
➜  ansible git:(master) ✗ scp -i ~/.ssh/swb-production-keypair.pem ~/.ssh/orchestra.md.crt ubuntu@ec2-54-226-179-206.compute-1.amazonaws.com:~/.ssh
orchestra.md.crt                                                                                                           100% 1859     1.8KB/s   00:00
➜  ansible git:(master) ✗ scp -i ~/.ssh/swb-production-keypair.pem ~/.ssh/orchestra.md.key ubuntu@ec2-54-226-179-206.compute-1.amazonaws.com:~/.ssh
orchestra.md.key

Thsi is the android keystore generated via: keytool -genkey -alias tomcat -keyalg RSA using the documented jboss password.

scp ~/.ssh/ryanvitalreactor-west.pem -i ~/.ssh/.keystore ubuntu@ec2-50-18-32-196.us-west-1.compute.amazonaws.com:~/ssh

## SSL setup

Staging will deploy SSL by default.. just run the main 2-install-and0confgis as uaual.

These scripts allow you to switch between SSL and non-SSL, but you do need to be aware of the order of operations.

if toggling SSL on or offyou should run update-all-application-code.yml

THEN you shoudl run update-proxy-nginx-config.yml

Note that there is a small amout of downtime in this since SSL is a core configuration change so it shoudl only be done in a very limited baseis.



## Advice

 run ./inv/ec2.py --refresh-cache if you ever get no hosts messages just to make sure you are workign wiht the latest invenory set.

 Use TMUX!! this way you can run things on the jumphost and quickly get back to your working session and work asynchronously




