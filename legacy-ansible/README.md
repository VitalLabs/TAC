

switchboard-infrastructure


New home of all ansible related files. Needed for engagement with AnsibleWorks and to get out of Code Commit History
=======
# Ansible Playbooks for Orchestra and Switchboard

- Requires Ansible 1.4 or later
- Expects Ubuntu (and maybe Debian) hosts

These Ansible scripts support deployment and configuration of a
Switchboard cluster and deployment of the Orchestra application to the
AWS CDN.

To configure credentials:
- Ensure vars/credentials.yaml values are filled in properly (copy from vars/credentials-example.yml)
- Add AWS credential to bin/aws_access and bin/aws_secret

To run Ansible, you will also need a properly configured shell:
- AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY defined for Boto
- ssh-agent running and loaded with the deploy user private key and swb-keypair.pem

We can shortcut this by:
- running 'source bin/ansible-env'

This configures the current shell with AWS credentials and appropriate keys loaded into SSH agent

## Unified Deployment

- Evaluation Node (create-/eval.yml) -- A single server test mode including
Cassandra, Datomic, Immutant, Switchboard, the Orchestra test server,
and Apache configuration, appropriate for routing requests to the
appropriate backend.

## Switchboard

- Staging Cluster (create-/stage.yml) -- A minimal cluster with 3-node Cassandra deployment sharing space with the Datomic transactor along with 2 Immutant nodes and a Jump Box.
- Production cluster (create-/production.yml) -- VPC, dedicated instances, two zone deployment, main and standby cassandra clusters, HA Datomic backed up by Cassandra, 2x2 Immutant cluster, and 2x mod_cluster load balancers.

## Orchestra

- Orchestra deploy (create-/orchestra.yml) -- Use local machine to build and deploy a build of the Orchestra platform to CloudFront.
