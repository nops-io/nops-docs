---
title: KMS Key Permissions
keywords: scheduler, storage, rightsizing, idle_resources, essentials, eventbridge, kms
tags: [essentials, scheduler, storage, rightsizing, idle_resources]
sidebar: mydoc_sidebar
permalink: essentials-kms-keys.html
folder: Essentials
series: [Essentials]
weight: 8.0
---

# Configuring KMS Key Permissions for nOps Essentials #

In this guide, we will walk you through the process of configuring KMS Key permissions for nOps Essentials. This feature allows nOps users to add KMS key permissions to their Lambda functions, enabling automated management of EC2 instances with encrypted EBS volumes. Particularly, this is required for two nOps Essentials features. First is the **Scheduler**, which lets you configure a start and stop time for your EC2, the other feature is the **Rightsizing**, which lets you optimize the size of your EC2.


## Prerequisites ##

Before you begin, ensure that the nOps Essentials stack is configured. This stack sets up the necessary Lambda functions for automations needed for all Essential features.


## Step-by-Step Guide ##

1. **Accessing the Configuration Feature**

   - Click on the menu icon in the top-right corner of the nOps dashboard.

   - Navigate to **Organization Settings** and select **Integrations**.

   - From the Integrations menu, choose **Event Bridge**.

2. **Reviewing Event Bridge Status**

   - In the Event Bridge list, identify the status of your configurations.

   - Yellow indicates "Update needed," red indicates "Not connected," and green indicates "Connected."

   - Ensure your Event Bridge status is green to proceed with KMS Key configuration.

3. **Choosing KMS Key Permissions**

   - Click the **Add KMS Keys** button next to a connected Event Bridge configuration.

   - A popup will appear with two options: **Use all KMS Keys** or **Single KMS Key**.

   - Select the appropriate option for your needs.

4. **Entering Single KMS Key (if applicable)**

   - If you select **Single KMS Key**, enter the KMS Key ARN from your AWS KMS console for the Key you would like to add.

   - Click **Launch Stack** to proceed.

5. **Launching the CloudFormation Stack**

   - After selecting your KMS Key options and clicking **Launch Stack**, you will be redirected to the AWS CloudFormation page.

   - Review the stack details and options.

   - Follow the on-screen prompts to launch the stack.

6. **Finalizing the Stack Launch**

   - Once the stack is successfully launched, the necessary permissions will be added to your Lambda execution role.

   - This setup will enable you to automate the management of EC2 instances with encrypted EBS volumes using nOps.


## How it Works ##

Once you choose the respective option for Single or All KMS Keys, a unique CloudFormation Launch Stack URL is prepared with the appropriate execution role for your Lambda function. Redirecting to the AWS CloudFormation page allows you to review the configuration options and proceed with the stack launch. Upon successful completion, the permissions required to manage EC2 instances with encrypted volumes are granted to your Lambda function. Our stacks are fully open source and can be reviewed [here](https://github.com/nops-io/nops-rules-lambda/blob/master/scheduler/cloudformation_templates/encrypted_ebs_kms_policy.yml) and [here](https://github.com/nops-io/nops-rules-lambda/blob/master/scheduler/cloudformation_templates/essentials_encrypted_ebs_kms_policy_all.yml). Both these stacks used for one key or all keys respectively applies following permissions to the Lambda hosted in your environment.

```json 
kms:RetireGrant
kms:CreateGrant
kms:ReEncrypt\*
kms:GenerateDataKey\*
kms:Encrypt
kms:DescribeKey
kms:Decrypt
```

## Benefits ##

This configuration provides flexibility, allowing users to grant their Lambda functions access to either all KMS keys or specific KMS keys as needed. This ensures secure and efficient automation of EC2 instance management with encrypted EBS volumes.


## Conclusion ##

Configuring KMS Key permissions in nOps is a straightforward process that significantly enhances the automation capabilities of your AWS infrastructure. By following this guide, you can ensure your Lambda functions have the necessary permissions to perform critical automation tasks securely.

<br/><br/>

{% include custom/series_related.html %}