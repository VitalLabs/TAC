# Ansible DevOps TODO list

Releases:
x Single-node evaluation
- Staging and production clustered solutions

## Specific Tasks

Organization:

- Move cluster .yml to subdir
- Helpful bash scripts
  - Setup agent/shell environment with creds
  - Create, destroy, update

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

- Apache/mod_cluster (auto-scaling keepalive group)
- How to register vars for client setup?


## Jump Box

JB Applications:

- Jumpbox SSH relay
- Ansible host w/ git scripts downloaded and credentials
- Apache
- Basic admin apache website
- SSL HTTP proxy for seeing interior resources
- 
- OpsCenter / DevCenter
- Nagios / monitoring?
- Datomic Console?
- Reimann / monit, etc.
- Other operational visualizations

## Future

- Immutant module for app mgmt?

