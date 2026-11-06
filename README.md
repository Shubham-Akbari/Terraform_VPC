# Terraform Multi-Tier VPC on AWS

Creates a VPC with a public and a private subnet using Terraform, verifies it in the AWS Console, then destroys it.

## Resources (8)

| Resource | Name | Detail |
|---|---|---|
| VPC | Student-VPC | 10.0.0.0/16 |
| Public subnet | Public-Subnet | 10.0.1.0/24 |
| Private subnet | Private-Subnet | 10.0.10.0/24 |
| Internet Gateway | Student-IGW | attached to the VPC |
| Route table | Public-RT | 0.0.0.0/0 to the IGW |
| Route table | Private-RT | no internet route |
| Associations (2) | public and private | one per subnet |

## Steps

### 1. Install Terraform and configure AWS
```bash
sudo apt install -y terraform
aws configure
```
Installs Terraform; `aws configure` stores the access key, secret key, region (`us-east-1`) and output format (`json`).



### 2. Create the project folder
```bash
mkdir terraform-vpc-lab
cd terraform-vpc-lab
```


### 3. main.tf
Contains the provider, VPC, subnets, Internet Gateway, route tables and associations.



### 4. Initialize
```bash
terraform init
```
Downloads the AWS provider and creates `.terraform.lock.hcl`.



### 5. Format and validate
```bash
terraform fmt
terraform validate
```

### 6. Plan
```bash
terraform plan
```
Dry run only: shows what will be created.



### 7. Apply
```bash
terraform apply
```
Type `yes` when prompted.



### 8. Verify in the AWS Console



The private subnet has no route to the Internet Gateway.

### 9. Clean up
```bash
terraform destroy
```
Type `yes` when prompted.

![terraform destroy: resources to be destroyed](images/15-terraform-destroy-plan.png)

![terraform destroy: 8 destroyed](images/16-terraform-destroy-complete.png)

> Remember to destroy the resources after the lab. A NAT Gateway or other resources added later can incur AWS charges.
