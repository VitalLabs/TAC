## Trusted analytics container

## Settings
All TAC settings are contained in the vars file `group_vars/tag_TAC_all_`:

You must export your AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY

    export AWS_ACCESS_KEY_ID='<SNIP>'
    export AWS_SECRET_ACCESS_KEY='<SNIP>'

You must create a seperate AWS keypair for TAC master access.

This is the setting `tac_keypair`.

### Credentials Storage

A `.gitignore` file keeps the directory `.ec2/` from being checked in.
Use that directory to store keyfiles, and/or environment files.

I use a file like this one `.ec2/tac-creds`:

    export AWS_ACCESS_KEY_ID='<SNIP>'
    export AWS_SECRET_ACCESS_KEY='<SNIP>'

### Networking

    cidr_block: 10.86.0.0/16
    subnets:
      - cidr: 10.86.0.0/24
        az: "{{ tac_zone }}"
    route_tables:
      - subnets:
          - 10.86.0.0/24
        routes:
          - dest: 0.0.0.0/0
            gw: igw

### VPC rules

    # rules
    tac_rules:
      - proto: tcp
        from_port: 22
        to_port: 22
        cidr_ip: 0.0.0.0/0
    
    tac_rules_egress:
      - proto: all
        cidr_ip: 0.0.0.0/0

### EC2 Instances

    # instances
    tac_instances:
      - instance_type: m1.medium
        image: "{{tac_imgid}}"
        keypair: "{{tac_keypair}}"
        instance_tags:
            Name: dev_tac
            dev_all: ""
            dev_tac: "master"
        exact_count: 1
        count_tag:
            Name: dev_tac_master

## Initial infrastructure 

To spin up a TAC EC2 instance use the following command:

    ansible-playbook -v 1-dev-TAC-infrastructure.yml -i inv/hosts --private-key <KEYFILE>

## Bootstrapping

The initial infrastructure playbook will call the bootstrapping playbook, but you can run it after the fact:

    ansible-playbook -v bootstrap-tac.yml -i inv/ec2.py --private-key <KEYFILE>
