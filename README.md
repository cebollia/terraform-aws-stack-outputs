# Cloudformation Stack Outputs

Creates a Cloudformation stack solely to use the Output or Export function for Terraform values. This allows
use to use Terraform for the bulk of your infrastructure, but use Cloudformation stacks for legacy inputs or
Serverless stacks. From the Cloudformation stack, you can use the !ImportValue function to retreive the necessary
value.

Since Cloudformation does not allow creating a stack with out atleast a single resource - to get around  
this, we create a custom `NullResource` in the Stack that uses a falsey condition, effectively creating a no-op.

This will not sanatize input, and fail if an unsupported character is passed in.

### Terraform Usage

```
module "stack-outputs" {

  source = "github.com/cebollia/terraform-aws-stack-outputs"

  name = "cloudformation-stack-name"
  outputs = {
    myVpcId = { 
      Value = aws_vpc.main.id
      Description = "Main AWS VPC"
      Export = {
        Name = "vpc:main:id"
      }
    }
    myVpcArn = {
      Value = aws_vpc.main.arn
      Description = "Main VPC ARN"
      Export = {
        Name = "vpc:main:arn"
      }
    }
  }

}
```
