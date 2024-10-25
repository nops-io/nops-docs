---
title: Onboarding Multiple AWS Linked Accounts with CloudFormation
keywords: setup, onboarding, tutorial, walkthrough, video
tags: [getting_started, onboarding]
sidebar: mydoc_sidebar
permalink: onboarding-aws-with-cloudformation.html
folder: GettingStarted
series: [Onboarding]
weight: 2.0
---


## Onboarding Multiple AWS Linked Accounts to nOps with CloudFormation ##

`nOps` requires safe, secure, and AWS-approved access to your AWS accounts in order to give you the analysis, dashboards, and reports that you need. We only see what you want us to see in order to provide our services, no more, and we need you to give us permission first.

In order to credential and register multiple accounts, we leverage _AWS Organizations_, _CloudFormation_, _Stack_, _StackSets_, and _Lambda_.

For multi-account setup, `nOps` recommends the use of CloudFormation.

<!--
**Watch the video on how to configure linked accounts with CloudFormation:** 

{% include video.html content="https://nops-docs-vids.s3.amazonaws.com/nOps-cloud-formation-stackset.mp4" vidclass="vid800600" vidwidth="100%" vidheight="100%" %}
-->



## Prerequisites ##

*   You must have Admin role permissions in AWS before you can add multiple AWS accounts to `nOps` using _CloudFormation_.

*   You have [configured your Payer account](onboarding-aws-with-automatic-setup).

*   Enable _Stackset_ in _AWS Organizations_ and _AWS CloudFormation_ within AWS.

*   Stackset Trusted Access must be enabled.
    

Once you’ve taken care of the prerequisites, the next steps are simple and straightforward.

## Adding Multiple AWS Accounts (CloudFormation) ##

If you don't have Stacksets enabled, or Trusted Access enabled, follow the steps below.

### Enable Stacksets ###

To enable _CloudFormation StackSets_ in _AWS Organizations_, go to **AWS Organizations / Services**. If you see **Access disabled** for _CloudFormation StackSets_, you will need to enable it.

Once enabled, you should see **Access enabled**:

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-aws-cloudformation-enabled.png)


### Enable Trusted Access ###
When navigating to **CloudFormation** **/** **StackSets**, you will be able to tell if Trusted Access is enabled.  If it's not, there will be a blue banner stating Tusted Access is not enabled.  Click to enable Trusted Access.  You may choose to disable it after configuration if you wish to do so.


## Create a Stackset for the Linked Accounts ##

To create and deploy a stackset for the linked accounts, make sure that you are logged into your AWS Management Account with Admin rights. The process to create the Stackset to casade down to all of your linked accounts, `nOps` will use a Cloudformation stack to configur the Stackset.

### Within the nOps Platform ###
To get started, you will need to be logged in as an admin in the `nOps` platform.

1. To the top right, navigate to **your login** **/** **Organization Settings**.
1. Click on the **_Cloudformation Multiple Accounts Setup_** button.
    ![](/tmpimg/CF-multiple-menu.png)
1. Confirm you are logged into your AWS Management account with admin rights, then click **_Proceed_**.
    ![](/tmpimg/cfmas_proceed.png)

### Within the nOps Platform ###
In the new tab that opens, everything is pre-filled for you in the Cloudformation Stack.  
1. Confirm the account you are logged into is the management account the Stack is deploying in.
1. Scroll down to the bottom and click **_Create Stack_**.

This process can take several minutes to complete.


{% include note.html content="It can take up to 24 hours before you start seeing the different `nOps` dashboards and compliance views populated with data from your workloads." %}

If you have any questions, please contact us at [help@nops.io](mailto:help@nops.io).



{% include custom/series_related.html %}