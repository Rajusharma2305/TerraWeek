1. Anatomy of a Block

General Syntax

block_type "label1" "label2" {
    argument = value
}
Example
resource "aws_instance" "web" {
  ami           = "ami-123456789"
  instance_type = "t2.micro"
}

Explanation

Part	Meaning
resource	Block Type
aws_instance	Label 1 (Resource Type)
web	Label 2 (Local Name)
{}	Block Body
ami = ...	Argument
instance_type = ...	Argument
Common Block Types
terraform {}

provider {}

resource {}

variable {}

locals {}

output {}

module {}

data {}
2. Difference Between Block and Argument
Argument

Arguments are key-value pairs.

Syntax

key = value

Example

instance_type = "t2.micro"

Another Example

volume_size = 20

Arguments always use =

Block

Blocks contain multiple configurations.

Example

resource "aws_security_group" "web" {

  ingress {
    from_port = 80
    to_port   = 80
    protocol  = "tcp"
  }

}

Here,

ingress {}

is a block.

Block vs Argument
Argument	Block
Uses =	Uses {}
Stores one value	Stores multiple settings
Example: ami = "ami-123"	Example: ingress {}
3. Expressions

Expressions calculate values dynamically.

Example

count = 2 + 3

Terraform stores

5

Another Example

name = upper("terraweek")

Output

TERRAWEEK
String Interpolation

Used to combine strings with variables.

Example

"${var.environment}-server"

If

environment = "dev"

Result

dev-server
References

Resources can reference other resources.

Syntax

resource_type.resource_name.attribute

Example

aws_instance.web.id

Meaning

Resource Type → aws_instance

Resource Name → web

Attribute → id

Another Example

aws_s3_bucket.bucket.arn
Operators
Arithmetic Operators
5 + 5
5 - 2
5 * 2
10 / 2
Comparison Operators
5 == 5

5 != 3

10 > 2

10 < 20
Logical Operators
true && false

true || false

!true
Task 2: Variables, Types & Validation

Variables make Terraform reusable.

Without variables

instance_type = "t2.micro"

With variables

instance_type = var.instance_type
Variable Syntax
variable "instance_type" {

  description = "EC2 Instance Type"

  type = string

  default = "t2.micro"

}

Use it

instance_type = var.instance_type
Primitive Types
String
variable "username" {
  type = string
}

Example

"Raja"
Number
variable "disk_size" {
  type = number
}

Example

20
Bool
variable "public_ip" {
  type = bool
}

Example

true
Collection Types
List

Ordered collection.

variable "regions" {
  type = list(string)
}

Example

["us-east-1","ap-south-1"]

Access

var.regions[0]
Map

Key-value pair.

variable "tags" {
  type = map(string)
}

Example

{
  Owner = "Raja"
  Team  = "DevOps"
}

Access

var.tags["Owner"]
Set

Stores only unique values.

variable "security_groups" {
  type = set(string)
}

Example

["web","db","web"]

Terraform stores

web

db
Structural Types
Object

Different named attributes.

variable "user" {

  type = object({

    name = string

    age = number

  })

}

Example

{
  name = "Raja"
  age  = 22
}

Access

var.user.name
Tuple

Fixed order and fixed data types.

variable "details" {

  type = tuple([
    string,
    number,
    bool
  ])

}

Example

["AWS",20,true]
Default Value
variable "environment" {

  type = string

  default = "dev"

}

If user doesn't pass anything,

Terraform uses

dev
Validation

Restricts user input.

variable "environment" {

  type = string

  default = "dev"

  validation {

    condition = contains(["dev","staging","prod"], var.environment)

    error_message = "Only dev, staging or prod allowed."

  }

}

If user enters

test

Terraform stops.

Sensitive Variable
variable "db_password" {

  type = string

  sensitive = true

}

Output

(sensitive value)

instead of showing the password.

Task 3: Locals, Outputs & Functions
Locals

Locals store calculated values.

Example

locals {

  name_prefix = "${var.environment}-web"

}

Use

local.name_prefix
Another Example
locals {

  common_tags = merge(

    {
      Environment = var.environment
    },

    var.tags

  )

}
Outputs

Outputs display information after

terraform apply

Example

output "instance_id" {

  value = aws_instance.web.id

}

Output

instance_id = i-123456789
Built-in Functions
upper()
upper("terraweek")

Output

TERRAWEEK
lower()
lower("AWS")

Output

aws
join()
join("-",["terra","week","2026"])

Output

terra-week-2026
merge()
merge(
  {Owner="Raja"},
  {Team="DevOps"}
)

Output

{
Owner="Raja"

Team="DevOps"
}
length()
length(["AWS","Docker","Terraform"])

Output

3
lookup()
lookup(
{
dev="small"
prod="large"
},
"prod"
)

Output

large
format()
format("%s-%d","server",1)

Output

server-1
Terraform Console

Run

terraform console

Examples

> upper("terraweek")

TERRAWEEK
> join("-",["tws","terraweek","2026"])

tws-terraweek-2026
> merge({a=1},{b=2})

{
a=1
b=2
}

Terraform Console is used to test expressions without creating infrastructure.

Task 4: Build Something Real (Docker Provider)
Step 1

Create the example folder.

example/
Step 2

Initialize Terraform

terraform init

Downloads the Docker provider.

Step 3

Run Terraform Plan

terraform plan \
-var="container_name=tws-web" \
-var="external_port=8080"

Terraform only shows what it will create.

Step 4

Apply

terraform apply \
-var="container_name=tws-web" \
-var="external_port=8080"

Terraform pulls the Nginx Docker image and starts the container.

Visit

http://localhost:8080
Step 5

Check Outputs

terraform output

Displays output values defined in outputs.tf.

Step 6

Destroy Resources

terraform destroy \
-var="container_name=tws-web" \
-var="external_port=8080"

Deletes the Docker container and cleans up all resources.

Using terraform.tfvars Instead of -var

Create a file named:

terraform.tfvars

Contents:

container_name = "tws-web"
external_port  = 8080

Now you can simply run:

terraform plan
terraform apply
terraform destroy

Terraform automatically loads values from terraform.tfvars, so you don't need to pass -var flags each time.
