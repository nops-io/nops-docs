---
title: Onboarding AWS Linked Accounts
keywords: setup, onboarding
tags: [getting_started, onboarding]
sidebar: mydoc_sidebar
permalink: onboarding-aws-linked-accounts.html
folder: GettingStarted
series: [Onboarding]
weight: 3.0
---

# Onboarding AWS Linked Accounts #

There are three methods of onboarding a linked account:

1.  [During automatic setup](#during-automatic-setup)
2.  [Via your nOps Organization Account](#nops-organization-settings)
3.  [With Terraform Multi Account Registration (IaaC)](#terraform-multi-account-registration-iaac)


All of these onboarding methods give the linked accounts IAM permissions that allow nOps to read metadata, CloudTrail trails, Cloudwatch data, and everything else about the linked accounts. It allows nOps to offer its monitoring and recommendations features for security, operations, reliability, and performance.

## During Automatic Setup ##

You can configure linked accounts during the automatic setup process of the payer account or at any time after the payer account has been configured. When you add an AWS Organization Payer Account during the automatic process, nOps will automatically pull in linked accounts associated with the payer account. 

After your Payer Account is configured successfully, the automatic setup will then ask you if you want to onboard the linked account(s) right now. You can click on **Automatic Setup** for each linked account to start the account onboarding process:

If you click on **Automatic Setup**, it will redirect you to the respective AWS account for you to create a stack that nOps will use to access the linked account. Please ensure that you are logged into the respective linked AWS account when you click **Proceed**:
![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-proceed.png)


When you click on Proceed, you will be redirected to **AWS > CloudFormation > Stacks > Create stack > Quick create stack** page, with most of the information pre-filled. Click on **Create Stack** to start the onboarding process.

During the onboarding process of linked accounts, nOps will not ask for the CUR since it has already been added with the AWS  Payer Account.

The setup process can take 1-2 hours to pull in data from AWS.

You can skip the onboarding of linked accounts during this setup and add the accounts later from within the [nOps Organization Settings](#nops-organization-settings) .

## nOps Organization Settings ##

If you decided to skip onboarding of the linked accounts during the Automatic Setup, you can still onboard your linked accounts via your nOps Organization Account.

Click on your account at the top right corner of the page and go to **Organization Settings > Cloud Accounts,** there you will see a list of linked accounts that nOps detected.

You can onboard each linked account with _Manual Setup_ or _Automatic Setup_:

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-nops-org-settings-account-list.png)

Click **Automatic Setup** or **Manual Setup** to start the onboarding process.

If you click **Automatic Setup**, it will redirect you to the respective AWS account for you to create a stack that nOps will use to access the linked account. Please ensure that you are logged into the respective linked AWS account when you click **Proceed**:

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-proceed.png)

When you click on Proceed, you will be redirected to **AWS > CloudFormation > Stacks > Create stack > Quick create stack** page, with most of the information pre-filled. Click on **Create Stack** to start the onboarding process.

If you click **Manual Setup**, you will be redirected to the Account Details (Manual Setup) page. Since nOps already has the information for S3 bucket that houses the CUR, the field for the S3 bucket will be locked. Click **Update Account** to start the onboarding process:

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-linked-account-manual.png)

During the onboarding process of linked accounts, nOps will not ask for the CUR since it has already been added with the AWS Organization Master Payer Account.

The setup process can take 1-2 hours to pull in data from AWS.

## Terraform Multi Account Registration (IaaC) ##

Use the Terraform Multi Account Registration process when, along with your AWS Organization Master Payer Account, you have numerous linked accounts that you want to onboard in nOps. This process makes it easier for you to onboard your linked accounts with minimal effort.

You can simply provide the Organizational Unit IDs (OUs) of your linked accounts during this setup and nOps will take care of the rest.

To learn about this onboarding process, see [Adding Multiple AWS Accounts to nOps with Terraform](onboarding-aws-with-terraform.html).


<br/><br/>

{% include custom/series_related.html %}