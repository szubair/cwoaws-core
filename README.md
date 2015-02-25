# cwoaws-core Ansible AWS magic - CWOAWS core Playbook
http://cwoaws.s3-website.eu-central-1.amazonaws.com/session4.html

Purpose of CWOAWS CORE Ansible playbook is to start full operational AWS VPC infrastructure, all services on EC2 instances, RDS, et.c and to allow later execution of separated Playbooks for deployments, etc.

CWOAWS CORE Ansible Playbook Tasks:
-  create 1 x VPC with 2 x VPC subnets in differrent AZ zones one AWS region
-  create 1 x security group
-  provision 2 x EC2 instance in 2 different AZ
-  Install webservers role on both instances (includes of other roles is possible webservers i required for instance ELB registration)
-  launch and configure public facing VPC ELB (cross_az_load_balancing) and attach VPC subnets
-  deregister and register EC2 instances on ELB.
-  info about ELB dnsname is writted in vars file and later can be used for DNS configuration

All informations about VPC-u are defined in main.yml andvvpc_info.yml file (AZ, regions, keypair, vpc, subnets, elb dns name info) 
.

ubuntu@ansible:~/cwoaws$ tree 
.
├── deploy-aws-infra.yml
├── ec2_web.yml
├── elb.yml
├── hosts
├── meta
│   └── main.yml
├── rds.yml
├── README
├── roles
│   ├── ec2_web
│   │   └── tasks
│   │       └── main.yml
│   ├── elb
│   │   └── tasks
│   │       └── main.yml
│   ├── rds
│   │   └── tasks
│   │       └── main.yml
│   ├── vpc
│   │   └── tasks
│   │       └── main.yml
│   └── webservers
│       ├── handlers
│       │   └── main.yml
│       └── tasks
│           └── main.yml
├── vars
│   ├── dev
│   │   ├── main.yml
│   │   └── vpc_info.yml
│   └── prod
│       ├── main.yml
│       └── vpc_info.yml
├── vpc.yml
└── webservers.yml

Installation

# Ansible and Boto installation Ubuntu 14.04 LTS
   $ sudo apt-get install software-properties-common
   $ sudo apt-add-repository ppa:ansible/ansible
   $ sudo apt-get update
   $ sudo apt-get install ansible
   $ apt-get install python-pip
   $ pip install boto

# Download to folder /etc/ansible/
   $ wget https://raw.github.com/ansible/ansible/devel/plugins/inventory/ec2.py
   $ wget https://raw.githubusercontent.com/ansible/ansible/devel/plugins/inventory/ec2.ini
   $ chmod +x /etc/ansible/ec2.py


# In aws console create IAM user with PowerUser privileges
# Add in file /home/ubuntu/.boto
   [Credentials]
   aws_access_key_id = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
   aws_secret_access_key = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

# Add in clean file /home/ubuntu/.ansible.cfg
   [defaults]
   host_key_checking = False
   private_key_file=/home/ubuntu/cwoaws/<keypair>.pem





