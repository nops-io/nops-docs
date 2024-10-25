<!--
---
title: Onboarding AWS with Manual Setup-Contact Us
keywords: setup, onboarding
tags: [getting_started, onboarding]
sidebar: mydoc_sidebar
permalink: onboarding-aws-with-manual-setup.html
folder: GettingStarted
series: [Onboarding]
weight: 9.0
---


# Setting up an AWS `nOps` account (Manual Setup) #


`nOps` requires safe, secure, and AWS-approved access to your AWS accounts in order to give you the analysis, dashboards, and reports that you need. We only see what you want us to see in order to provide our services, no more, and we need you to give us permission first.


In order to get started with `nOps`, the first step is to set up an AWS account for `nOps` via the [Automatic Setup](onboarding-aws-with-automatic-setup.html), Manual Setup, [IaaC Multi Account Setup (CloudFormation)](onboarding-aws-with-cloudformation.html), or IaaC Multi Account Setup (Terraform). We made the setup process as easy as possible for you while complying with AWS security best practices.


This Manual Setup is used in complex environments by experienced AWS administrators who need granular control and insight into the access that `nOps` require.


The Manual Setup approach is also useful for administrators who want to embed `nOps` access into their automation.


## Prerequisites ##


You must have Admin role permissions in the AWS Payer account before you can set up an AWS `nOps` account with Manual Setup.


**Pro Tip:** The Manual Setup is used in complex environments by experienced AWS administrators. Most customers opt to use the Automatic Setup procedure.


## Configuring the AWS Payer account (Manual Setup) ##




To use the Manual Setup, follow these steps:


1.  [Get the auto-generated External ID from nOps](#get-the-auto-generated-external-id-in-nops)
2.  [Setup a S3 billing bucket for Cost & Usage Reports](#setup-s3-billing-bucket-for-cost--usage-reports)
3.  [Give nOps Permission and Create an IAM Policy](#create-the-payer-account-iam-policy)
4.  [Create an IAM Role](#creating-the-iam-role)
5.  [Return to nOps to complete Manual Setup](#return)


{% include important.html content="If you need any help with this process don’t hesitate to contact [help@nops.io](mailto:help@nops.io)"%}


### Important information to copy and save ###


During this process, you should copy and save some information as you will need to enter it during the setup process. This information will be used in AWS and in `nOps` in order to complete the process:


* Copy the **External ID** auto-generated through `nOps`.
* Copy the **ARN** for **IAM Policy** that was created in the IAM Policy.
* Copy **Report name** created for the Cost and Usage Report (CUR).
* Copy **Report path prefix** from the S3 billing bucket creation.


## Get the auto-generated External ID in nOps ##


When you log in to your `nOps` account for the first time, a pop-up screen will appear. This pop-up screen will guide you on how you can add your AWS account(s) to `nOps`. The screen consists of three distinct sections:


1.  Getting Started
2.  Link Cloud Accounts
3.  Fetching


{% include note.html content="If you only add a single account during the setup and want to add more accounts later, once your single account is onboarded and you have access to the `nOps` platform:<br /><br />




1. On the top-right corner of your `nOps` account, click on your user avatar to open a drop-down list.<br />
2. In the dropdown list, click **Organization Settings**. This will take you to the **Cloud Accounts** page.<br />
3. To the right of the account you wish to configure, click your preferred setup option.


"%}


## Getting Started ##


In this section, you need to select the account setup method. In the scope of this article, we will deal with the **Manual Setup**. Select the **Manual Setup** and click **Next**.


When you click **Next,** you should see a screen similar to:
<span id="externalid">
![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-aws-manual-setup-fields.png)</span>


Copy the **External ID** and save it, you will need it later on.


**Important:** Do not exit this page, you will return to this page later to complete the account setup.


## Setup S3 billing bucket for Cost & Usage Reports ##


This section is divided into two steps, in the first step you will create the Cost & Usage Report, and in the second step you will create/select an S3 bucket for the Cost & Usage Report.


{% include note.html content="Ensure that your AWS SCP configurations allow IAM administrators to make the changes."%}


### Create the Cost & Usage Report ###


In this step you will create a Cost & Usage Report (CUR) so that `nOps` can analyze your cost information:


1.  Login to your AWS Management Console account.
2.  Go to the [Cost & Usage repports](https://console.aws.amazon.com/billing/home?#/reports)
3.  Click on **Create Report:**
   ![](https://lh7-us.googleusercontent.com/F9npughBt18j9fn47qiNAPI2MPmaFa5izZbMPvvCCcc4SlHfNXAQngo5WryhyeUkaJvS4HAD2PfTtdCgAA9ju8nH87KeqZa_uWsGxRDLNoF08e6IcydfLZQXULwunG0y29al4DA1sjJ6AXPIE1dFrkk)


4.  Create a **report name** (such as _nopsbilling-hourly-parquet_).
5.  In **Additional report details**, check the **Include resource IDs** checkbox (mandatory).
6.  In the **Data refresh settings**, leave the **Refresh automatically** checkbox checked.
7.  Click **Next**.


When you click **Next**, it will take you to the **Delivery options** page where you will create the S3 billing bucket.


### Create/Select the S3 billing bucket ###




AWS needs a place to save your cost and usage report. In this step, you will create an S3 bucket that secures your information:


1.  In the **Delivery options**, click **Configure** to open the **Configure S3 Bucket** dialog box.
2.  In the dialog box, do one of the following: 
   **Select an existing bucket**: Use an existing bucket from your AWS Account. 
   or 
   **Create a new bucket:** Create a new S3 bucket to be used specifically for `nOps`.
   ![](https://lh7-us.googleusercontent.com/zCnJHNmYqly5e-JwX3vshB8TIzJU6BXOSh6IfFT0rnMEA_GfXYoUyLafm4YIuAj2_3LFm5bkasCI6yrgib8mLIB2RdwhZmSc2vO1vYl-8eteinTe8OGbNSeClMUtJCfAvnVoE72kDAJZQiFQiH7yPd0)
  
   Check the "The following default policy will be applied to your bucket" checkbox


3.  Click **Save**.
4.  In the **Report Delivery Options** details:
   ![](https://lh7-us.googleusercontent.com/YYv6FnRSyooyV_6lsMXwguJHr5EkyksDGvi6dzFgyfcw520hF7prDfZIuBYS4s_zsmP9tiT07IG69C4XxUM7LL7CBOrwnyLSTcnzr1DbYj8Rqd_2yG9B1-5zzJyDNjH4XqX0I_C0SGT8yYU9MEn2DIU)




   * Enter the **report path prefix** (required) – Suggestion: _nopsbilling_
   * Choose **Hourly** (mandatory) for **Time granularity**.
   * Select an option for **Report versioning** (optional) — Overwrite existing report.
   * Select **Parquet** as **Compression type** (mandatory). 
       **Important:** You will need the **Report Path Prefix name** later when you are adding the AWS Account in `nOps`


5. Click **Next**.


6. In the **Review and Complete** page, double-check your settings.
7. Click **Create Report**.




## Create the Payer account IAM policy ##




In this step, you’ll give `nOps` permission to read the Cost & Usage Report in the S3 bucket.


{% include important.html content="


AWS has a sophisticated security system for Identity and Access Management (IAM). There are no shortcuts for this. The `nOps` [Wizard/Automatic Setup](onboarding-aws-with-automatic-setup.html) makes this easier with a CloudFormation Template, but the details provided in this article are for AWS practitioners who need more information for their own automation or auditing purposes.


"%}


To manually create the IAM policy in order to allow `nOps` appropriate access:


1.  On the AWS Management Console, go to the **Identity and Access Management** screen.
2.  From the left navigation panel, click **Policies**.
3.  Click **Create Policy**.
4.  Switch to the **‘JSON’** tab and replace the existing JSON script with the script provided in [nOps IAM Policy](iam-policy-nops-free-platform.html#payer-account-iam-policy-json).
5.  Click **Next: Tags** (optional).
6.  Click **Next: Review**.
7.  Click on **‘Review Policy’**.
8.  **Copy and save the ARN of the IAM Policy.** This will be used later when you create the **IAM Role**.
9.  Provide a name and description for the policy.
10. Click on ‘**Create Policy**’.




Make sure you replace &lt;paste-bucket-name-here&gt; with the name of the S3 bucket that houses the Cost & Usage Report to ensure policy efficacy.


You will attach this to the IAM Role that you will create for `nOps` in the next step.


## Creating the IAM role ##


**IMPORTANT:** You will need to enter the [nOps auto-generated ID](#externalid) to create the IAM Role.


In order to allow the `nOps` SaaS application to use the IAM policy you just created, you need to create an IAM role.


To create a new role:


1.  On the AWS Management Console, go to the **Identity and Access Management** screen.
2.  From the left navigation panel, click **Roles**.
3.  Click **Create Role**.
4.  On **Select trusted entity** page, select **AWS account.**
5.  Click **Another AWS account.**
6.  For **Account ID** enter the `nOps` account ID (202279780353).
   ![](/tmpimg/manual_trusted_entity.png)


7.  Click **Require external ID**.
8.  For **External ID**, enter the [external id](#externalid) that was auto-generated for you by `nOps`.
9.  Click **Next**.
10. In **Add permissions**, check the box for the IAM policy you just created.
11. Click **Next**.
12. Enter a name and description for the role.
13. (Optional) **Add tags**.
14. Click **Create Role**.
15. In the role list, find and click on the role you just created.
16. Find and copy the **Role ARN** in the Summary page for the role and save it.


You have now completed the first part of the Manual Setup related to the AWS console.


## Continue the Manual Setup of AWS account in nOps ##


Now that you have manually configured an IAM Role in your AWS account for access to AWS resources, the last step is to add that account to `nOps`.


You must now add information about the AWS role, S3 bucket, and CUR information so that `nOps` can fetch the billing data and asset information.


{% include note.html content="If you do not add a S3 bucket, your billing stats pages in `nOps` will not display any data."%}


1.  Start from where you left off in [Create an auto-generated External ID from nOps](#get-the-auto-generated-external-id-in-nOps) section.
2.  In the **Account Details (Manual Setup)** page, enter a name for the AWS account you are adding to `nOps`.
3.  The External ID is auto-generated.
4.  Enter the ARN of the IAM role.
5.  Add the S3 bucket name. Make sure the S3 bucket name is the **_same_** as the S3 bucket you created or selected for your CUR file.
6.  Enter the name of the Cost & Usage Report you created in step 4 of [Create the Cost & Usage Report](#create-the-cost--usage-report) section.
7.  Enter the report prefix path that you created in step 6 of [Create/Select the S3 billing bucket](#createselect-the-s3-billing-bucket).
8.  Click **Verify Details**.
   ![](https://nOps-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-aws-manual-setup-fields.png)




## Link Cloud Accounts ##


`nOps` will check the account connectivity with AWS, and start the ingestion.




## Fetching ##


After the initial data validation is complete, the setup process will automatically pull in the child accounts associated with your Organization account.


To onboard a child account, click either **Automatic Setup** or **Manual Setup**. If you don’t want to add a specific child account, click **Proceed and Setup later**


See [Onboarding Linked Accounts](onboarding-aws-linked-accounts.html) and [Onboarding With CloudFormation](onboarding-aws-with-cloudformation.html) for more information about configuring Linked Accounts.




<br/><br/>


{% include custom/series_related.html %}

-->