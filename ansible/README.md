##Dependencies for AWS use:


Export your AWS credentials as usual:

   export AWS_ACCESS_KEY_ID="****"
   export AWS_SECRET_ACCESS_KEY="******"
   ssh-add ~/.ssh/keypair.pem


You MUST use a stable version of ansible. 1.5.0 for right now.

pip install ansible==1.5.0

You must have AWS CLI installed (NOT the older EC2 CLI) and configured.  We use shell commands to augment where core EC2 modules don't support actions.

If you will be using dedicated VPC instances then we will need to use a patched module. We will ping ansible when that time comes.

Then cd into this playbook directory and run as needed.



##Proper commands to run playbooks:

First Provision EC2 instances

ansible-playbook -vvvv -i ./inv/ 1-dev-infrastructure.yml

Then install all core software and apply configurations. Right nwo both are together. We can break this out into a AMI image build in the future.

bootstrap the jumphost to run the deployments:

ansible-playbook -vvvv -i ./inv/ bootstrap-jumphost,yml

Install everything:

ansible-playbook -vvvv -i ./inv/ 2-install-and-configs.yml


## Ad Hoc Playbooks:

Eaxmple: ansible-playbook -vvvv -i ./inv/ depploy-app-deploy.yml

deploy-app-deploy.yml:  This installs the Switchboard app via the app-deploy role and restarts the server.

deploy-app-clojure-config.yml:  This installs the Orchestra app via the clojure-app role and restarts the server.

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

 this is where we will keep adding one off commands as DevOps workflows evolve


 ## JumpHost Key setup:

 You will need to export your 
 AWS access keys AND ssh-agent/ ssh-add your key that ansible is configure to use.  Keeping this step manual for now since keys should be local and secure.

  Fast Testing using Jumphost:

 you can ssh to the jumphost to run these scripts. Since they are on the same betwork they will run alot faster.

 /var/lib/swb-ansible contains the latest deployment scripts.

 ## Advice

 Use TMUX!! this way you can run things on the jumphost and quickly get back to your working session and work asynchronously




