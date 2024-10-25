<!--
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

Here are the methods of onboarding a linked accounts:

1.  [With Stacket Multi Account One Click Setup](#With-Stacket-Multi-Account-One-Click-Setup)
2.  [During automatic setup](#during-automatic-setup)
3.  [Via your `nOps` Organization Account](#`nOps`-organization-settings)
4.  [With Terraform Multi Account Registration (IaaC)](#terraform-multi-account-registration-iaac)


All of these onboarding methods give the linked accounts IAM permissions that allow `nOps` to read metadata, CloudTrail trails, Cloudwatch data, and everything else about the linked accounts. It allows `nOps` to offer its monitoring and recommendations features for security, operations, reliability, and performance.

## With Stacket Multi Account One Click Setup ##

Onboarding Multiple AWS Accounts to `nOps` using One Click CloudFormation

**Introduction**

`nOps` provides powerful analysis, dashboards, and reports for your AWS environment. Adhering to the principle of least privilege, `nOps` requires limited access to your AWS accounts via AWS-approved methods including AWS Organizations, CloudFormation, Stack, StackSets, and Lambda.

This guide will walk you through the process of onboarding multiple AWS child accounts to `nOps` using a one-click CloudFormation setup.

**Prerequisites**

AWS Organization Master Payer account or access to the Master Payer account for configuration.

Note: Before initiating, keep the AWS Organization’s Master Payer account signed in on another tab of your browser.
 
Skip Step: 1 if the Master Payer account is already configured.

**Steps**

Step 1: Add a New AWS Account
	Note: we are going to add the master payer AWS account 

1. Initiate Account Addition:
If you have created a new `nOps` client you will be asked to configure AWS on the very first Sign-in or Click on "Add New AWS Account" from the Dashboard if your account is new, or add it from Personal Settings > AWS Accounts.

    <img src="https://nOps-help-site-assets.s3.amazonaws.com/OneClickCloudformation/pic-1.png">


2. Select "`nOps` Wizard Setup" and click "Next".

    <img src="https://nOps-help-site-assets.s3.amazonaws.com/OneClickCloudformation/add+new+account.png">

3. Enter Account Details:
Enter the AWS Account Name and S3 Bucket Name.
Click "Set up Account". This will redirect you to the AWS Create Stack page.


4. Create Stack in AWS:
The Create Stack page will have prefilled data including Stack name, ExternalId, and SystemBucketID.
Check the box that says "I acknowledge that AWS CloudFormation might create IAM resources."
Click the "Create Stack" button.
The stack will be created and the Master Payer account will be configured successfully this may take up to 10 minutes after successful stack creation.

 Step 2: Configure All Linked Child Accounts 

1.  After the Master Payer account is successfully configured, it will automatically fetch the linked child AWS accounts and list them as Inactive AWS Accounts.

2. Initiate MultiAccount Setup:
To configure all child accounts at once, click on "CloudFormation Multiple Accounts Setup" from the AWS Accounts page, which will redirect you to the AWS Create Stack Page.

    <img src="https://nOps-help-site-assets.s3.amazonaws.com/OneClickCloudformation/pic-4.png">

3. Create Stack for Multiple Accounts:
A Create Stack page will open on AWS with the name "Quick Create Stack".and All details would be prefilled.


    <img src="https://nOps-help-site-assets.s3.amazonaws.com/OneClickCloudformation/pic-5.png">

Click on the "Create Stack" button to initiate the stack run.

After successful CloudFormation execution within 30 minutes, all child accounts will be connected to `nOps` as Active AWS Accounts.


    <img src="https://nOps-help-site-assets.s3.amazonaws.com/OneClickCloudformation/pic-6.png">


This feature also supports automatic listing and connection of any new child accounts created in AWS to `nOps` so you don't need to manually configure any new AWS account created under the same organization account.

**Conclusion**
By following these steps, you can seamlessly onboard multiple AWS-linked accounts to `nOps`, granting minimum necessary permissions through AWS-native CloudFormation.


## During Automatic Setup ##

You can configure linked accounts during the automatic setup process of the payer account or at any time after the payer account has been configured. When you add an AWS Organization Payer Account during the automatic process, `nOps` will automatically pull in linked accounts associated with the payer account. 

After your Payer Account is configured successfully, the automatic setup will then ask you if you want to onboard the linked account(s) right now. You can click on **Automatic Setup** for each linked account to start the account onboarding process:

If you click on **Automatic Setup**, it will redirect you to the respective AWS account for you to create a stack that `nOps` will use to access the linked account. Please ensure that you are logged into the respective linked AWS account when you click **Proceed**:
![](https://nOps-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-proceed.png)


When you click on Proceed, you will be redirected to **AWS > CloudFormation > Stacks > Create stack > Quick create stack** page, with most of the information pre-filled. Click on **Create Stack** to start the onboarding process.

During the onboarding process of linked accounts, `nOps` will not ask for the CUR since it has already been added with the AWS  Payer Account.

The setup process can take 1-2 hours to pull in data from AWS.

You can skip the onboarding of linked accounts during this setup and add the accounts later from within the [`nOps` Organization Settings](#`nOps`-organization-settings) .

## `nOps` Organization Settings ##

If you decided to skip onboarding of the linked accounts during the Automatic Setup, you can still onboard your linked accounts via your `nOps` Organization Account.

Click on your account at the top right corner of the page and go to **Organization Settings > Cloud Accounts,** there you will see a list of linked accounts that `nOps` detected.

You can onboard each linked account with _Automatic Setup_:

![](https://nOps-help-site-assets.s3.amazonaws.com/OneClickCloudformation/orgSetting.png)


Click **Automatic Setup** to start the onboarding process.

Once you click **Automatic Setup**, it will redirect you to the respective AWS account for you to create a stack that `nOps` will use to access the linked account. Please ensure that you are logged into the respective linked AWS account when you click **Proceed**:

![](https://nOps-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-proceed.png)

When you click on Proceed, you will be redirected to **AWS > CloudFormation > Stacks > Create stack > Quick create stack** page, with most of the information pre-filled. Click on **Create Stack** to start the onboarding process.


During the onboarding process of linked accounts, `nOps` will not ask for the CUR since it has already been added with the AWS Organization Master Payer Account.

The setup process can take 1-2 hours to pull in data from AWS.

## Terraform Multi Account Registration (IaaC) ##

Use the Terraform Multi Account Registration process when, along with your AWS Organization Master Payer Account, you have numerous linked accounts that you want to onboard in `nOps`. This process makes it easier for you to onboard your linked accounts with minimal effort.

You can simply provide the Organizational Unit IDs (OUs) of your linked accounts during this setup and `nOps` will take care of the rest.

To learn about this onboarding process, see [Adding Multiple AWS Accounts to nOps with Terraform](onboarding-aws-with-terraform.html).


<br/><br/>

{% include custom/series_related.html %}

-->
