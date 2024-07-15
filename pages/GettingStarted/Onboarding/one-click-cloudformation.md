---
title: Onboarding AWS with 1-click CloudFormation
keywords: setup, onboarding, tutorials, video, walkthrough, one click, cloudformation
tags: [getting_started, onboarding]
sidebar: mydoc_sidebar
permalink: one-click-cloudformation.html
folder: GettingStarted
series: [Onboarding]
weight: 1.0
---


# Onboarding Multiple AWS Accounts to nOps using One Click CloudFormation #

## Introduction ##

nOps provides powerful analysis, dashboards, and reports for your AWS environment. Adhering to the principle of least privilege, nOps requires limited access to your AWS accounts via AWS-approved methods including AWS Organizations, CloudFormation, Stack, StackSets, and Lambda.

For multi-account setups, nOps recommends using CloudFormation over Terraform due to its better integration with AWS services and native support. This guide will walk you through the process of onboarding multiple AWS child accounts to nOps using a one-click CloudFormation setup.

## Prerequisites ##

- Administrative access to the AWS Management account account for configuration.
- Another tab of your browser opened with nOps

 

{% include note.html content="Skip Step: 1 if the Master Payer account is already configured."%}

## Steps ##

### Step 1: Add a New AWS Account ###

1\. Initiate Account Addition:

Click on "Add New AWS Account" from the Dashboard if your account is new, or add it from **Personal Settings** > **AWS Accounts**.

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXfBndsnJtQ221a15KMJUR-nKYcwL7FLaj7QzIk6NB5eR89Ye_9IPESRx2LyJ8cX2GV4DzatlF7JPy9Gm5ts46vj8wcWpatnia9KB1hfLg8TLC6qJNJFKYHMOKrjNS7isOiQrbtimZpneva5gNjMs_rFviDy?key=doRlbOCgYPAnjy2xgnYNEg)

2\. Choose Setup Method:

We provide two setup methods: 

     1. nOps Wizard Setup

     2. Manual Setup

For this guide, select "nOps Wizard Setup" and click "Next".

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXcSchvWlHdwL2VTzuCp7ysmR-nNnuE9wWsxl4bPEfTkNbflQF-Bj2W_l04lZABDrGqDqDvTl4-0ibuDwOhTAW8X9BtFZt-0fknxSy64DJxa7GRAkho84xq3MIwa0O4S7eoSsthYNJeFCHwSsPDwX-sFn8i8?key=doRlbOCgYPAnjy2xgnYNEg)

3\. Enter Account Details:

- Enter the AWS Account Name and S3 Bucket Name.

- Click "Set up Account". This will redirect you to the AWS Create Stack page.

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXc0wylFLwkyOGf9H_sW8xtWxHXL-faudTl4R8o_T4jzuKAlg4ktp-Q9rjVUKrDixrYy5FjHpd05f-Dbg0aKOzvkW1xOFeT0d3380EwA70NVOs1FSW8y05LA5GoH-QnxKgqIEmhdpNtPf7OBTClVHaYfje5T?key=doRlbOCgYPAnjy2xgnYNEg)

4\. Create Stack in AWS:

- The Create Stack page will have prefilled data including Stack name, ExternalId, and SystemBucketID.

- Check the box that says "I acknowledge that AWS CloudFormation might create IAM resources."

- Click the "Create Stack" button.

- The stack will be created and the Master Payer account will be configured successfully.

### Step 2: Configure All Linked Child Accounts ###

1.  After the Master Payer account is successfully configured, it will automatically fetch the linked child AWS accounts and list them as **Inactive AWS Accounts.**

2\. Initiate MultiAccount Setup:

- To configure all child accounts at once, click on "**CloudFormation Multiple Accounts Setup**" from the AWS Accounts page, which will redirect you to the AWS Create Stack Page.

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXdAKXHInrYeRyMk5AMCA8aqzFu-xdMP7UU1Rilk8B6BhFfJSsang0KdrFloOHWdT657LeFURPTE5842XO2b3Yqg_qQAVaN3NY664TfL1b17lS2sUvW4ulQCfzzXsfw3oy1pdFa13OVNimK_GcuBP2Nqd0YH?key=doRlbOCgYPAnjy2xgnYNEg)

3\. Create Stack for Multiple Accounts:

- A Create Stack page will open on AWS with the name "Quick Create Stack".and All details would be prefilled.

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXemMfVCDPpFSkkcZB3Xn39H1RPipCCXeyXAOT4m7h6IAE0waAEUqYH3IlBCGEB_0YBeFMra5XAmSer1kl75JYABv9IoYA43YoERF-qInxfdza3OBo47_RLNRDk9fcpu06SxiWgR29HO6cfr5EzDo9s3b11I?key=doRlbOCgYPAnjy2xgnYNEg)

- Click on the "Create Stack" button to initiate the stack run.

- After successful CloudFormation execution, all child accounts will be connected to nOps as **Active AWS Accounts**.

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXf3dYS15APhxx7dGtaXmOUw496wSAk_AsKMM6Mj1xK8ep0psCkHL--7jKj6Wjd7WkpV9ihBNW8ADjIx0Wse52IzYS96L8ziuu4HGkJlqmATXahAEnnTgsX5uvmVhxnFbrlncV08BhBWeId09_kUr_bA9Rh7?key=doRlbOCgYPAnjy2xgnYNEg)

This feature also supports automatic listing and connection of any new child accounts created in AWS to nOps so you don't need to manually configure any new AWS account created under the same organization account.

## Conclusion ##

By following these steps, you can seamlessly onboard multiple AWS-linked accounts to nOps, granting minimum necessary permissions through AWS-native CloudFormation.


<br/><br/>

{% include custom/series_related.html %}