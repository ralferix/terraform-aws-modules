# Terraform Test
Background
This is to test your understanding of infrastructure as a code. Port numbers can be arbitrary, as long as they all line up. We should be able to generate a valid plan using an AWS provider; however the infrastructure does not need to work to the point of being spun up. Feel free to make up any necessary details as need be.
Task & Requirements
Using the AWS Provider, build the following:
Fleet of application server instances with AWS ELB
Fleet of worker server instances
Aurora database cluster (MySQL Engine, at least 3 instances)
These should be contained in a VPC with the appropriate security groups and subnets.
Bonus:
Sensible directory and file structure
Provide for mapping cloud assets via AWS API (tags)
Cloudfront
Terraform modules
"Precompilation" of terraform code files to allow for more advanced topology patterns.
Other creative solutions/ideas

# Complete VPC
Plan: 54 to add, 0 to change, 0 to destroy.

Configuration in this directory creates set of VPC resources which may be sufficient for staging or production environment (look into [simple-vpc](../simple-vpc) for more simplified setup).

There are public, private, database, ElastiCache, intra (private w/o Internet access) subnets, and NAT Gateways created in each availability zone.

## Usage

To run this plan you need to execute:

```bash
$ terraform init
$ terraform plan
$ terraform apply
```

Note that this plan may create resources which can cost money (AWS Elastic IP, for example). Run `terraform destroy` when you don't need these resources.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Outputs

| Name | Description |
|------|-------------|
| database\_subnets | List of IDs of database subnets |
| elasticache\_subnets | List of IDs of elasticache subnets |
| intra\_subnets | List of IDs of intra subnets |
| nat\_public\_ips | List of public Elastic IPs created for AWS NAT Gateway |
| private\_subnets | List of IDs of private subnets |
| public\_subnets | List of IDs of public subnets |
| redshift\_subnets | List of IDs of redshift subnets |
| vpc\_endpoint\_ssm\_dns\_entry | The DNS entries for the VPC Endpoint for SSM. |
| vpc\_endpoint\_ssm\_id | The ID of VPC endpoint for SSM |
| vpc\_endpoint\_ssm\_network\_interface\_ids | One or more network interfaces for the VPC Endpoint for SSM. |
| vpc\_id | The ID of the VPC |

# Complete ELB

Configuration in this directory creates ELB, EC2 instances and attach them together.

This example also creates ACM SSL certificate which can be attached to a secure listener in ELB.

Data sources are used to discover existing VPC resources (VPC, subnet and security group).

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| number\_of\_instances | Number of instances to create and attach to ELB | string | `"1"` | no |

## Outputs

| Name | Description |
|------|-------------|
| this\_elb\_dns\_name | The DNS name of the ELB |
| this\_elb\_id | The name of the ELB |
| this\_elb\_instances | The list of instances in the ELB (if may be outdated, because instances are attached using elb_attachment resource) |
| this\_elb\_name | The name of the ELB |
| this\_elb\_source\_security\_group\_id | The ID of the security group that you can use as part of your inbound rules for your load balancer's back-end application instances |
| this\_elb\_zone\_id | The canonical hosted zone ID of the ELB (to be used in a Route 53 Alias record) |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
