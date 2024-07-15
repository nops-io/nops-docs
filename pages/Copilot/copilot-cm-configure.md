---
title: Configure Commitment Management
keywords: savings, recommendations, sharesave, savings plans, commitment management
tags: [copilot]
sidebar: mydoc_sidebar
permalink: copilot-cm-configure.html
folder: Copilot
series: [Copilot, Savings]
weight: 2.0
---

# Commitment Management Configuration #


If you're prefer to walk through configuring **Commitment Management** with nOps, please contact your nOps Sales representative.

If you'd prefer to configure on your own, please follow the steps outlined below.

## Prerequisites ##

1. Access to the management account of the AWS Organization
1. Ability to create and access a new email account or address


## Create an email address ##

In order to enroll in Commitment Management, you will need to create one email address for this purpose under the company domain, such as nops-cm@company.com.  If your email service uses aliasing, that will work too.  We recommend yourname+cm@company.com in that situation.

Follow your internal process to get access to your **nops-cm@** email account.

## Add a new AWS account to the AWS Organization ##

Login to your AWS management/payer account and follow the steps.

1. Navigate to the AWS Organization page.
2. Click on **Add an AWS account**

![](https://lh7-us.googleusercontent.com/TiMSUjBRAHGYowheZ0HyR_hpucdPL8pPQCN6R3zjKIFnws3CUrzQn0YuuBfA48VcFaJ5B3ts3VJxL6n7cb8_7PJlsjqD_i9X7rvMpxnrpSdCqRJ6YRfZt8hNtItLs3BV1GAtkd6ckLAHSTDd1XSCA38)

3. Add account information (we recommend ShareSave or Commitments for the name) using the new email address and click **Create AWS account**

![](https://lh7-us.googleusercontent.com/YEUNG3gXDprDLEYQQiDp_T7jBKTIbpApqE8QAE1TuM5XPpS4DdgPHm6xbilOOFEk8G6ruP34roFXl2neHQYVMwMThMP3AttDQX8pW6V8Eyi-Ms0OYUCVTuybOPvhqeQVR5TcwtlazPiwo6j3aagZpjw)

## Configure the Payer roles ##
While the account creation processes, run the following [CloudFormation stack](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=nops-sharesave-setup&templateURL=https://s3-external-1.amazonaws.com/cf-templates-1o8svzqapyba5-us-east-1/2022095DAy-nops-sharesave-setup4moad088den)

```
https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=nops-sharesave-setup&templateURL=https://s3-external-1.amazonaws.com/cf-templates-1o8svzqapyba5-us-east-1/2022095DAy-nops-sharesave-setup4moad088den
```

You will need to enter 2 variables.


| **Report name**: |  **Bucket**: |
| --- | --- |
| _company_-cm | _company_-cm |



By now, you should receive confirmation the new account has been created.


### Create a Password for the new AWS account ###

1. Open [aws.amazon.com](https://aws.amazon.com)
1. Log in as a root user email
    1. Enter your nops-sharesave@company.com email
    1. Click **Next**
    1. Click the _Forgot password?_ link
1. Reset your password


### Configure the new linked account ###

After logging into the linked account, run the following [CloudFormation stack](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=nops-sharesave-roles&templateURL=https://s3-external-1.amazonaws.com/cf-templates-1o8svzqapyba5-us-east-1/2023353p12-nops_sharesavev2qp14lq7vf3)

```
https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=nops-sharesave-roles&templateURL=https://s3-external-1.amazonaws.com/cf-templates-1o8svzqapyba5-us-east-1/2023353p12-nops_sharesavev2qp14lq7vf3
```

When this has completed and the stacks have run successfully, please contact nOps with the following information:

- Payer account number
- ShareSave linked account number
- ShareSave bucket name

And we can complete the configuration process on a short call.

{% include custom/series_related.html %}