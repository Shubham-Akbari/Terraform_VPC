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

![Terraform installed (v1.6.3)](images/01-terraform-installed.png)

![aws configure (region us-east-1, output json)](images/02-aws-configure.png)

### 2. Create the project folder
```bash
mkdir terraform-vpc-lab
cd terraform-vpc-lab
```
![Create the project folder and main.tf](images/03-create-project-folder.png)

### 3. main.tf
Contains the provider, VPC, subnets, Internet Gateway, route tables and associations.

![main.tf content](images/04-main-tf.png)

### 4. Initialize
```bash
terraform init
```
Downloads the AWS provider and creates `.terraform.lock.hcl`.

![terraform init: provider installed](images/05-terraform-init.png)

### 5. Format and validate
```bash
terraform fmt
terraform validate
```
![terraform fmt and validate: configuration is valid](images/06-terraform-fmt-validate.png)

### 6. Plan
```bash
terraform plan
```
Dry run only: shows what will be created.

![terraform plan: resources to be created](images/07-terraform-plan-resources.png)

![terraform plan: 8 to add, 0 to change, 0 to destroy](images/08-terraform-plan-summary.png)

### 7. Apply
```bash
terraform apply
```
Type `yes` when prompted.

![terraform apply: 8 resources added](images/09-terraform-apply.png)

### 8. Verify in the AWS Console

![AWS Console: Student-VPC is Available](images/10-aws-console-vpc.png)

![AWS Console: VPC CIDR 10.0.0.0/16](images/11-aws-console-vpc-cidr.png)

![AWS Console: Public 10.0.1.0/24 and Private 10.0.10.0/24 subnets](images/12-aws-console-subnets.png)

![AWS Console: Student-IGW attached to Student-VPC](images/13-aws-console-internet-gateway.png)

![AWS Console: Public-RT and Private-RT associated with subnets](images/14-aws-console-route-tables.png)

The private subnet has no route to the Internet Gateway.

### 9. Clean up
```bash
terraform destroy
```
Type `yes` when prompted.

![terraform destroy: resources to be destroyed](images/15-terraform-destroy-plan.png)

![terraform destroy: 8 destroyed](images/16-terraform-destroy-complete.png)

> Remember to destroy the resources after the lab. A NAT Gateway or other resources added later can incur AWS charges.
