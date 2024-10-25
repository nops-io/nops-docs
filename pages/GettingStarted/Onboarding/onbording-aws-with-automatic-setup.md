---
title: Onboarding AWS with Automatic Setup
keywords: setup, onboarding, tutorials, video, walkthrough
tags: [getting_started, onboarding]
sidebar: mydoc_sidebar
permalink: onboarding-aws-with-automatic-setup.html
folder: GettingStarted
series: [Onboarding]
weight: 1.0
---

# Adding your AWS payer account to nOps with Automatic Setup #

`nOps` requires safe, secure, and AWS-approved access to your AWS accounts in order to give you the analysis, dashboards, and reports that you need. We only see what we need, no more, and we need you to give us permission first.

In order to get started with `nOps`, the first step is to subscribe to `nOps` on the AWS marketplace.  Next, you'll create an `nOps` login and set up your AWS payer account. We've made the setup process as easy as possible for you while complying with AWS security best practices.

In Automatic Setup, `nOps` takes care of creating the S3 bucket, Cost and Usage Report (CUR), and IAM policy through a CloudFormation stack.

**Watch the video on how to configure your Payer account with automatic setup:** 

{% include video.html content="https://nops-docs-vids.s3.amazonaws.com/nops_free_setup.mp4" vidclass="vid800600" vidwidth="100%" vidheight="100%" %}

## Prerequisites ##
To successfully set up the AWS account(s), the AWS user must possess:

* An accepted AWS Marketplace offer.  
    {% include important.html content="Contact sales@nops.io if you do not have an MPPO." %}
* Access to the Payer account, if you are using AWS Organizations.
* Permission to create and run an AWS CloudFormation stack.
* Permission to create AWS Identity and Access Management (IAM) roles in your account.
* The name of an Amazon S3 bucket where your AWS Cost and Usage Reports (CURs) will be written. (`nOps` will create a bucket with the provided name if one does not exist)
* CURs enabled in the organization.

{% include note.html content="If you add an AWS child account instead of a Payer Account, `nOps` will only see the cost details of the specific child account instead of the cost details of the entire organization."%}


## Onboarding the AWS Payer Account ##

When you log in to your `nOps` account for the first time, a pop-up screen will appear. This pop-up screen will guide you on how you can add your AWS Payer account to `nOps`. The screen consists of three distinct sections:

1.  [Getting Started](#getting-started)
2.  [Link AWS Accounts](#link-aws-accounts)
3.  [Fetching](#fetching)

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-onboardingwizard.png)

{% include note.html content="
After adding your Payer account with the automatic setup, to add more accounts later:<br /><br />

1.  On the top-right corner of your `nOps` account, click on your user avatar to open a drop-down list.<br />
2.  In the dropdown list, click <b>Organization Settings</b>. This will take you to the <b>Cloud Accounts</b> page.<br />
3.  On the <b>Cloud Accounts</b> page, click <b>Automatic Setup</b> for the account you wish to configure. <br />
"%}

## Getting Started ##

In this section, you need to select the account setup method. In the scope of this article, we will deal with the **Automatic Setup**. Select the **nOps Wizard Setup** and click **Next**.


To learn more about **Manual Setup**, see [Manual Setup](onboarding-aws-with-manual-setup.html). To learn more about **IaaC Setup**, see [Onboarding with CloudFormation](onboarding-aws-with-cloudformation.html).


## Link AWS Accounts ##

In the case of an **AWS Organization** account:

* Make sure that you are logged into your AWS Master Payer Account.
* Select the _AWS Organization_ option.
* Fill out the _AWS Master Payer Account Name_ and _S3 Bucket Name_ fields.
* Click **Setup Account.**

If you select **AWS Organization** account, in the next section **Link Cloud Account**, you will have the option to onboard the child accounts associated with your _AWS Organization Account_.

In the case of the **Payer Account**:

* Make sure that you are logged into your AWS account with admin rights.
* Select the _Single Account_ option.
* Fill out the _AWS Account Name_ and _S3 Bucket Name_ fields.
* Click **Setup Account**.

When you click **Setup Account**, you will be redirected to your **AWS** \> **Create Stack** page. All the fields on this page will be pre-populated. Click on the checkbox for **“I acknowledge that AWS CloudFormation might create IAM resources”**. `nOps` needs this permission to automate the creation of the IAM role.

After you click the checkbox, click on the **Create** button to start the data ingestion.

{% include note.html content="CloudFormation stacks can run from any region you prefer. You can easily change the region of the CF stack from the **CloudFormation** screen once you launch it from nOps."%}



Once the stack is created, come back to `nOps`. `nOps` will check the account connectivity with AWS and check the CloudFormation stack permissions, and start the ingestion:

When data ingestion starts, in the AWS console **CloudFormation > Stacks > Stack Detail**:

1.  If you have all the required permissions, as mentioned in the prerequisites section, the setup will start creating the stack with the status **“CREATE\_IN\_PROGRESS”**. Once the stack is created the **“Status”** will change to **“CREATE_COMPLETE”**. You can click the browser refresh button to check progress. Normally it takes 1 to 2 minutes to complete the process.
2.  If you don’t have proper permissions then you will see errors as shown in the screenshot below, and the stack will not be created. You can assign the necessary permissions to the AWS user or ask other teammates to rerun the setup.
3.  Once the stack creation is successful, log in to [nOps Dashboard](https://app.nops.io/) after the `nOps` integration (stack) creation process is completed

## Fetching ##


Once your AWS accounts are linked successfully, you will see the following screen:

![](https://nOps-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-aws-automatic-fetching.png)

Once you log back into `nOps`, after data ingestion is complete, in the case of AWS _Organization Account_ you will see the **Setup Child Account** page. With the help of your CUR, the setup process will automatically pull in the child accounts associated with your organization account.

To onboard a child account, click the **Automatic Setup** button against the child account that you want to add. If you don’t want to add a specific child account, click **Skip Setup.**


{% include note.html content="If you don’t have the required permissions to onboard a child account, click **Invite team member** to invite a member of your organization who has the required permissions."%}


If you click **Automatic Setup**, the setup process will show you a confirmation popup.

Before you click **Proceed**, make sure that you are logged in to the child account you are onboarding. When you click **Proceed,** you will be redirected to the AWS CloudFormation console with all the fields pre-filled:

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-aws-automatic-stackset.png)

Check the **I acknowledge that AWS CloudFormation might create an IAM resources** checkbox, and click **Create Stack**.

To take a look at the `nOps` CloudFormation template, see [CloudFormation YAML Template](https://s3-us-west-2.amazonaws.com/nops-users/nOpsRole-uat.yaml).

In case, `nOps` detects more than 10 child accounts, you will get a prompt to use the [nOps Terraform Setup](onboarding-aws-with-terraform.html). `nOps` recommends that in this case, you use the [CloudFormation Setup](onboarding-aws-with-cloudformation.html) instead of the **Automatic Setup**. 

Once all the linked accounts are added or skipped, click **Next.**

The setup process is now complete.  It can take up to 24 hours before you start seeing the different `nOps` dashboards and compliance views populated with data from your workload.

{% include important.html content="If you have any questions, please contact us at [help@nops.io](mailto:help@nops.io)"%}

On initial ingestion, `nOps` will pull the data from AWS accounts based on the following durations:

* Cost data: 6 months look back + current month.
* Rules: Current date.
* CloudTrail Events: 14 days look back.

## IAM and CloudFormation: ##


The IAM policy used by `nOps` is scoped to read and write permissions only.

Lambda function automates the creation of Role and Bucket (if it’s absent) for `nOps` integration to work.

The code for the Lambda function is available for your review. Click the link to get the [YAML file](https://s3-us-west-2.amazonaws.com/nops-users/nOpsRole-uat.yaml).

If you are not comfortable with using the automated setup, you can use the [manual setup](onboarding-aws-with-manual-setup.html) instead.


## Troubleshooting Tips: ##

* Do you have a **pop-up blocker** on your browser? A pop-up blocker on your browser will stop `nOps` from redirecting you to an AWS account to create a stack.
* There may have been a disconnect when creating the S3 stack causing the stack to have an error of **ROLLBACK_ERROR.** In this case, re-try the automatic setup, then delete the first one.
* Is it pulling in incorrect data? Make sure that you are logging into the correct account. When you have multiple access to AWS accounts, it can import the wrong data. Ensure that you’re logged in to the correct account prior to starting the integration process.
* If you belong to an Organization ( multiple accounts linked to a Master Account) ensure that you are logged into the Master account before running the wizard (so the billing data is populated) or having organizational billing data files exported to one of your buckets.

{% include custom/series_related.html %}