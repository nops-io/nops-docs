---
title: Onboarding AWS Accounts to nOps with Terraform
keywords: setup, onboarding, terraform
tags: [getting_started, onboarding]
sidebar: mydoc_sidebar
permalink: onboarding-aws-with-terraform.html
folder: GettingStarted
series: [Onboarding]
weight: 8.0

---

## Onboarding AWS Accounts to nOps with Terraform ##

nOps requires safe, secure, and AWS-approved access to your AWS accounts in order to give you the analysis, dashboards, and reports that you need. We only see what you want us to see in order to provide our services and we need you to give us permission first.



**Prerequisites**


* Admin role permissions in AWS in order to add the AWS Payer and/or linked accounts to nOps using _Terraform_.

* And API key generated from the nOps platform.
  


## nOps Onboarding ##


When you log in to your nOps account for the first time, a pop-up screen will appear. This pop-up screen will guide you on how you can add your AWS account(s) to nOps:
    ![](/tmpimg/onboard_start.png)

1. Select the IaaC Multiple Accounts Setup option and click **Next**.
    ![](/tmpimg/terraform_radio.png)
1. This section informs you of the prerequisites needed to complete the process. To generate an API key, click **Proceed to Create API Key**.

    ![](/tmpimg/terraform_intro.png)

1.  In the **Generate new API key** section
    ![](/tmpimg/terraform_generatingkey.png)
    
    Enter:
    * An API key name
  
    * API Key description
  
1. After you add all the information, click **Generate**.


{% include note.html content="nOps will generate and display an API key for you. Copy and save the API key for future use, and click **OK**." %}

At this point, it's time to move to Terraform to finish the process.

## Terraform for AWS Linked Accounts ##

The nOps provided public Terraform module contains the complete logic to quickly and seamlessly onboard AWS accounts into the nOps platform. 
Customers using Terraform as the IaC of choice can quickly integrate their account into nOps by calling this module.
The module can be found in the [Terraform registry](https://registry.terraform.io/modules/nops-io/nops-integration/aws/latest), refer to it for additional details.


## Features ##

- Automatic detection of existing nOps projects for the AWS accounts
- Creation of new nOps projects if none exist
- Handling of payer and linked AWS accounts
- Automatic setup of IAM roles and [policies](iam-policy-nops-free-platform.html) for nOps integration
- S3 bucket creation and configuration for payer accounts
- Integration with nOps API for secure token exchange


## Prerequisites ##

- Terraform v1.0+
- AWS CLI configured with appropriate permissions
- nOps API key


## Onboarding Management account ##

The example below shows how to add the management (root) AWS account integration:


1. Being authenticated on the Payer account of the AWS organization, add the following code:
    ```hcl
    provider "aws" {
      alias  = "root"
      region = "us-east-1"
      assume_role {
        role_arn = "arn:aws:iam::123456789012:role/admin-role"
      }
    }
    
    module tf_onboarding {
      providers = {
        aws = aws.root
      }
    
      source             = "nops-io/nops-integration/aws"
      # This bucket will be created by the module with the name provided here, make sure its globally unique.
      system_bucket_name = "example"
      # nOps API key that will be used to authenticate with the nOps platform to onboard the account.
      api_key            = "nops_api_key"
    }
    ```

3. Initialize Terraform:

    ```hcl
    terraform init
    ```

4. Plan and apply the Terraform configuration:

    ```hcl
    terraform apply
    ```

5. If you want to reconfigure an existing nOps account:
    ```hcl
    terraform apply -var="reconfigure=true"
    ```

    or

    ```hcl
    module tf_onboarding {
      providers = {
        aws = aws.root
      }
    
      source             = "nops-io/nops-integration/aws"
      system_bucket_name = "example"
      api_key            = "nops_api_key"
      reconfigure        = true
    }
    ```

{% include note.html content="The `reconfigure` flag is required after the first onboarding flow with the master account, this flag is used to allow updates to the current project. Make sure to add this flag after the first run." %}

After your Terraform apply has finished, your account should list within the nOps platform as payer.

## Onboarding Child accounts ##

Onboarding child accounts is achieved using the same module, it already contains the logic to react when its being applied on any account that is not root.

```hcl
provider "aws" {
  alias  = "child"
  region = "us-east-1"
  assume_role {
    role_arn = "arn:aws:iam::xxxxxxxx:role/admin-role"
  }
}

module tf_onboarding {
  providers = {
    aws = aws.child
  }

  source             = "nops-io/nops-integration/aws"
  # This bucket will be created by the module with the name provided here, make sure its globally unique.
  system_bucket_name = "example"
  # nOps API key that will be used to authenticate with the nOps platform to onboard the account.
  api_key            = "nops_api_key"
  # Flag required to update the nOps project.
  reconfigure        = "true"
}
```

## Troubleshooting ##

- If you run this with an existing role, you may see an error like:

{% include warning.html content="Error: creating IAM Role (NopsIntegrationRole-xxxxx): EntityAlreadyExists: Role with name NopsIntegrationRole-xxxxx already exists." %}

At which point you can run the command with the reconfigure variable set to true.

- You can import the role to terraform state by running the following command

    ```hcl
    terraform import aws_iam_role.nops_integration_role NopsIntegrationRole-xxxxx
    ```


<br/><br/>

{% include custom/series_related.html %}