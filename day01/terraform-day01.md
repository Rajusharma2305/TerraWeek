TerraWeek Day 1 - Infrastructure as Code (IaC) & Terraform Basics
1. What is Infrastructure?

Before learning Terraform, you need to know what infrastructure means.

Infrastructure refers to all the resources required to run an application.

For example, if you're deploying a web application, you might need:

Virtual Machine (EC2)
Storage (S3 Bucket)
Database (RDS)
Network (VPC)
Load Balancer
Security Groups

Without these resources, your application cannot run.

2. What is Infrastructure as Code (IaC)?

Imagine your manager says:

"Create an EC2 instance."

Method 1: Manual (Without IaC)

You open AWS Console.

Then you click

AWS Console

↓

EC2

↓

Launch Instance

↓

Select Ubuntu

↓

Choose t2.micro

↓

Create Key Pair

↓

Choose Security Group

↓

Launch

This may take 10-15 minutes.

Tomorrow you need another identical server.

You repeat all the steps again.

Next week another teammate creates the server.

He accidentally chooses:

Ubuntu 24 instead of Ubuntu 22
t3.micro instead of t2.micro
Wrong security group

Now both servers are different.

This creates problems.

Problems with Manual Method

❌ Takes time

❌ Human mistakes

❌ Hard to repeat

❌ Difficult to track changes

❌ No version control

❌ Cannot easily recreate infrastructure

IaC Solution

Instead of clicking buttons,

You write a file.

Example:

resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
}

Now Terraform reads this file and creates the server automatically.

No clicking.

Just run:

terraform apply

Done.

Real Life Example

Without IaC:

You cook every meal from memory.

Sometimes you forget salt.

Sometimes more spice.

Every meal becomes different.

With IaC:

You have a recipe.

Every person following the recipe makes exactly the same dish.

Infrastructure is also created exactly the same way every time.

Advantages of IaC
Automation
Repeatability
Faster deployments
Version control using Git
Team collaboration
Easy disaster recovery
Less human error
Easy rollback
3. What is Terraform?

Terraform is an Infrastructure as Code tool developed by HashiCorp.

Instead of manually creating infrastructure,

You describe what you want.

Terraform creates it.

Example

You write

resource "aws_s3_bucket" "demo" {
  bucket = "my-demo-bucket"
}

Terraform creates the bucket.

Why is Terraform Popular?
1. Declarative

You tell Terraform

"I want one EC2 instance."

You don't tell it

click here
select Ubuntu
press Launch

Terraform figures out how.

2. Provider Agnostic

Terraform supports many platforms.

One tool can manage:

AWS
Azure
GCP
Docker
Kubernetes
GitHub
Cloudflare
VMware
Oracle Cloud

Same syntax.

Different providers.

3. Huge Ecosystem

Thousands of providers.

Thousands of reusable modules.

Large community.

DevOps Workflow

Typical DevOps pipeline:

Developer writes code

↓

Push to GitHub

↓

GitHub Actions runs

↓

Terraform creates infrastructure

↓

Docker builds image

↓

Kubernetes deploys application

Terraform is responsible for provisioning infrastructure.

4. Terraform vs Other Tools
Terraform vs OpenTofu

OpenTofu is an open-source fork of Terraform.

Terraform:

Created by HashiCorp
Business Source License (BSL)

OpenTofu:

Community maintained
Fully open source

Most Terraform code works in OpenTofu.

Terraform vs Pulumi

Terraform:

Infrastructure is written in HCL.

Example

resource "aws_instance" "web" {}

Pulumi:

Infrastructure is written in programming languages.

Example

Python

aws.ec2.Instance(...)
Terraform vs CloudFormation

CloudFormation works only on AWS.

Terraform works on:

AWS

Azure

GCP

Docker

Kubernetes

GitHub

Cloudflare

etc.

Terraform vs Ansible

Terraform creates infrastructure.

Example

Create

EC2
VPC
S3

Ansible configures servers.

Example

Install

Nginx
Docker
Java

Terraform says:

"I created the server."

Ansible says:

"I installed software inside the server."

Many companies use both together.

5. Install Terraform

Download from HashiCorp.

Check installation.

terraform version

Example output

Terraform v1.15.x

Help command

terraform -help

Shows all commands.

Like

init

plan

apply

destroy

validate

VS Code Extension

Install

HashiCorp Terraform Extension

Benefits

Syntax highlighting
Auto complete
Error detection
Formatting
6. Six Important Terraform Terms
Provider

A provider is a plugin that lets Terraform communicate with a platform.

Examples

AWS Provider

Docker Provider

Azure Provider

GitHub Provider

Example

provider "aws" {
  region = "ap-south-1"
}

Terraform now knows

"I'll work with AWS."

Resource

A resource is anything Terraform creates.

Examples

EC2

S3 Bucket

Docker Container

GitHub Repository

Example

resource "aws_s3_bucket" "bucket" {
  bucket = "mybucket"
}
State

Terraform keeps a record of everything it created.

That record is stored inside

terraform.tfstate

Without state,

Terraform forgets what it created.

Think of it like a notebook where Terraform writes:

"I created this EC2."

"I created this bucket."

Plan

Before creating anything,

Terraform shows you what will happen.

Command

terraform plan

Output

+ create EC2

+ create S3 Bucket

Nothing is created yet.

It is only a preview.

HCL

HCL means

HashiCorp Configuration Language.

This is the language Terraform uses.

Example

resource "aws_instance" "web" {
  instance_type = "t2.micro"
}

It is easy to read compared to JSON or XML.

Module

A module is reusable Terraform code.

Suppose every project needs

VPC
EC2
Security Group

Instead of writing everything again,

Create a module once.

Reuse it.

Think of it as a function in programming.

7. Your First Terraform Configuration

The example uses:

Local Provider

Creates files on your computer.

Random Provider

Generates random values.

No AWS account needed.

No charges.

Step 1
terraform init

Purpose

Downloads providers.

Creates

.terraform/

and

terraform.lock.hcl

Think of it like:

npm install

or

pip install
Step 2
terraform fmt

Formats your code.

Before

resource"local_file""demo"{

After

resource "local_file" "demo" {

Makes code readable.

Step 3
terraform validate

Checks

syntax
missing values
configuration errors

Does not create anything.

Step 4
terraform plan

Shows

+ create file

+ generate random string

Preview only.

Step 5
terraform apply

Actually creates the resources.

You'll be asked

Do you want to perform these actions?

yes

Type

yes
Step 6
cat greeting.txt

Reads the generated file.

Example

Hello Raja
Step 7
terraform destroy

Deletes everything Terraform created.

This prevents unnecessary cloud costs.

8. Terraform Workflow
Write Code
      │
      ▼
terraform init
      │
      ▼
terraform fmt
      │
      ▼
terraform validate
      │
      ▼
terraform plan
      │
      ▼
terraform apply
      │
      ▼
Infrastructure Created
      │
      ▼
terraform destroy
Why this order?
Write: Create your .tf files describing the desired infrastructure.
Init: Download the required providers and initialize the working directory.
Fmt: Automatically format your Terraform code for readability and consistency.
Validate: Catch syntax or configuration errors before making changes.
Plan: Review exactly what Terraform intends to create, modify, or destroy.
Apply: Execute the approved changes and provision the infrastructure.
Destroy: Remove all resources managed by Terraform when they're no longer needed
