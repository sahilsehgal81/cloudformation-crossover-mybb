
# AWSCloudFormation Infrastructure

#                                   
#  NOTE: Videos has been created and attached under Demo directory 
#                                                             

## Overview

This is a fully functional AWS CloudFormation template (along with additional required source codes
residing in the public GitHub repository https://github.com/sahilsehgal81/cloudformation-crossover-mybb) for
running the MyBB application on a scalable, highly-available and secure infrastructure.

### Running the stack

To run this project in an AWS Account do the following:

- Create an EC2 KeyPair (required for SSH access, can't be automated by CF);
- Launch a stack from this template with CloudFormation;
- Go to the URL in the "BBBalancerDNSName" output variable for the live MyBB application.

## Evaluation Access Account

- AWS Console access:
    - URL: https://473107760050.signin.aws.amazon.com/console 
    - Username: crossover
    - Password: Crossover@123$
    - Notes:
        - Currently there is **1 stacks** deployed in region **us-east-1 (N. Virginia)**.
        - Read-only access granted for: **CloudFormation, EC2, RDS, S3, SNS and CloudWatch**.

- MyBB application Administrator Account:
    - Username: admin
    - Password: 1234

- If you need any other access please [contact me](mailto:sahilsehgal81@gmail.com)!

Assumptions:

This project deploys a Multi Tier Architecture on a single AWS region. The region is intentionally not explicitly specified in the
template so the region in which the stack is launched will be used.

I assumed an externally registered domain name (not via Route53). The automation template outputs the AWSgenerated DNSName of the main (BB) load balancer and a CNAME record should be defined to point to this name.

Prior to the launch of this template into a stack, you will have to create an EC2 KeyPair in the AWS account (for SSH
access).

What's Left:

The current template does not cover uploading user generated files to an S3 bucket and delivering them via CloudFront.

The current state of the automation template reflects what I managed to build in the time I had available, and so there are a few
aspects which could (and probably should) be improved before going live with it (note that some are more important than others).

Add S3/CloudFront support for uploaded files: I intentionally did not use S3FS for storage and retrieval of uploaded files.

I would definitely avoid this option if possible and use the AWS PHP SDK to write the files straight to S3. Here's why:

Using a FS abstraction is more difficult to control and debug, errors are less likely to be visible and we can't enforce
retries or other type of error handling.

Reading those files would take them from S3, through the EC2 machines to the user; this complicates the architecture
badly, will perform badly and denies use of CloudFront.

I've taken a look over the MyBB codebase and it should not be difficult to add support for S3 file uploads, which easily
extends to support downloading files via CloudFront CDN. This would perform and scale better.

#Issues faced while completing the assignment:

I had to work on my company's projects too, therefore I tried to wrap it up as fast as I could.
