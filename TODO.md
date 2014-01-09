# Ansible DevOps TODO list

Immediate:
- Configure orchestra to use external CORS address
- Test production mode on eval server (fixes above)

- Use IAM roles if can create from scripts (?)
- Use git-encrypt for sensitive data
- Change cassandra config back to default (from 19xxx)

Releases:
x Single-node evaluation
- Staging and production clustered solutions

## Specific Tasks

Organization:

- Helpful bash scripts?
  - Setup agent/shell environment with creds
  - Create, destroy, update, etc.
- Better cluster lifecycle:
  - xxx-build: Build/configure cluster AMIs (how to save AMI refs?)
  	- Netflix AMINATOR
	- Docker
  - xxx-launch: Launch nodes, configure service links, enable services
  - xxx-shutdown: Shutdown cluster
  - xxx-update: online lifecycle upgrades
	- OS security patches (new AMI in AWS)
	- Services updates (new AMI in AWS?)
    - Application updates (new WAR)
  - xxx-upgrade: Rolling upgrade using new AMI
  	- Migrations?

Baseline:
- Auto-install security patches?

AWS Integration:
- Every cluster has a single user authority
- Use IAM roles to configure instances?
  
DSE/Cassandra:

- Ephemeral disks and RAID0
- Encrypted RAID0 volume
- Encryption key distribution solution
- Client/Server SSL security
- Client/Server authentication
- Automated backups (Direct EBS or Hadoop/S3?)

Datomic:

- Support for the DDB Store
- Support for the Cassandra Store
- More flexibility between free and dev modes?
- Backup processes (S3)

Immutant:

- Separate leiningen role
- Ensure explicit java dependency from role meta
- Custom configuration
  - S3_PING or AWS_PING for clustering
  - HornetQ clustered or SQS/SNS?
  - mod_cluster jboss configuration

Switchboard:

- Github hooks for redeploy?

Orchestra:

- Github hooks for redeploy?

Proxy front-end:

- Test out mod_cluster on eval
- Auto-scaling keepalive group for LB?
- How to register vars for client setup?


## Jump Box
- AutoScaling group

JB Applications:

- Jumpbox SSH relay / SOCKS Proxy
- Ansible host w/ git scripts downloaded and credentials
- AnsibleWorks AWX w/ Github hooks for deploy, etc?
- Apache
  - Basic admin apache website
- OpsCenter / DevCenter
- Nagios / monitoring?
- Datomic Console?
- Reimann / monit, etc.
- Other operational visualizations


# Future Efforts

- Immutant module for app mgmt? (use jboss)
- Docker containers for databases, jumpbox apps, etc
- SE Linux support?

