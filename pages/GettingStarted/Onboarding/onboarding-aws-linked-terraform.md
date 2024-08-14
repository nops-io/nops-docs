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
  
* Access to the nOps public Github repository [nOps Cloud Account Registration](https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform).

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

nOps AWS Integration Terraform Module

[https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform#nops-aws-integration-terraform-module](https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform#nops-aws-integration-terraform-module)


## Description ##

[https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform#description](https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform#description)

This Terraform module automates the process of integrating your AWS account(s) with nOps, a cloud management and optimization platform. It streamlines the setup of necessary AWS resources and permissions, enhancing the onboarding experience for nOps users.


## Features ##

[https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform#features](https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform#features)

- Automatic detection of existing nOps projects for the AWS accounts
- Creation of new nOps projects if none exist
- Handling of payer and linked AWS accounts
- Automatic setup of IAM roles and [policies](iam-policy-nops-free-platform.html) for nOps integration
- S3 bucket creation and configuration for payer accounts
- Integration with nOps API for secure token exchange


## Prerequisites ##

[https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform#prerequisites](https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform#prerequisites)

- Terraform v0.13+
- AWS CLI configured with appropriate permissions
- nOps API key


## Usage ##

[https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform#usage](https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform#usage)

1. Clone [this repository](https://github.com/nops-io/nops-integration/tree/main/platform/aws_setup_terraform):

2. Create a `terraform.tfvars` file with your specific variables:

    ```hcl
    aws_region = "us-west-2"
    api_key    = "your-nops-api-key"
    system_bucket_id = "your-system-bucket-id"
    ```

3. Initialize Terraform:

    ```hcl
    terraform init
    ```

4. Plan and apply the Terraform configuration:

    ```hcl
    terraform apply
    ```

5. If you want to allow the role to be reconfigured:
    ```hcl
    terraform apply -var="reconfigure=true"
    ```


After your Terraform apply has finished, your accounts should list within the nOps platform.

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