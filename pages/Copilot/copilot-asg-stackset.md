---
title: Onboarding your Autoscaling Groups to nOps Compute Copilot via Stackset
keywords: savings, recommendations, sharesave, asg, copilot, autoscaling
tags: [copilot, asg]
sidebar: mydoc_sidebar
permalink: copilot-asg-stackset.html
folder: Copilot
series: [Copilot, Onboarding]
weight: 5.0
---


# Configuring Multiple AWS Linked Accounts with CloudFormation for ASG #

Using CloudFormation, it’s easy to automatically onboard and configure multiple linked AWS accounts to nOps. 

Creating a [StackSet](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-concepts.html) in your AWS Management (Master) Account will deploy Stack Instances in all child accounts automatically. It allows you to save time by configuring many accounts simultaneously (following the service-managed permissions model). 


## More information on service-managed permissions ##

With service-managed permissions, you can deploy stack instances to accounts managed by AWS Organizations. Using this permissions model, you don't have to create the necessary IAM roles; StackSets creates the IAM roles on your behalf. With this model, you can also turn on automatic deployments to accounts that you add to your organization in the future.


## How to configure via StackSet ##

### Prerequisites

- You must have an **Admin role** in your **AWS Master Account**.
- Navigate to **AWS Organizations > Services > CloudFormation StackSets** and enable access for **CloudFormation StackSets**

![](https://lh7-us.googleusercontent.com/xc1Nun3L75e_X2-RM2WgIlu1hVspfQBFxQfRVdaN82nB92MYuPZW64uGUgTT4OrobSu6GnzRRTeITuu75cgi3RJzC6QGRanXtg7EGdLYqs0U0KcaOxxu7TECERbQR-AzHqkVPV3CKA0H45UU6SZKzfs)

## Step-by-step guide


### Step1: API Key Generation

- Log in to your **nOps** dashboard.
- Navigate to **Organization Settings** **> API key > Generate New API Key.**
- Name the API key as desired. For example, “**compute-co-pilot-asg”.**
- Save the key as we will be using it shortly.


![](https://lh7-us.googleusercontent.com/sivo4QqRUDjh9qYfwgiINlGaACt2FMmyKZC1Cj_Bf9V18ogdyRerK2SvmRK8lhRlT1yUq7ZAFetv2GqRU61KSFsQXuOlakQn5TcFzEPEtMa1hCmwpIn6Lkbn45H5eCvpvgwlGVVDD_L6mNiGgV5KNj0)

### Step 2: Deploying StackSet

- Log in to your **AWS Master Payer account** with admin permissions.
- From within the **AWS Console > CloudFormation > Stacksets page**, click **Create Stackset**.
- In the **Specify template** section, choose **Amazon S3 URL**. 


Add the following S3 URL: **https://nops-prd-asg-lambda-us-east-1.s3.us-east-1.amazonaws.com/v0.7.2/cloudformation/lambda-v0.7.2.yaml**


![](https://lh7-us.googleusercontent.com/BBQWD1rQA4d2DGO3dFwUgIEgBfg30PauFszqDZO4em0eGXdv3QNBFvggVJhiTVyo_J_Iz-UVxykZ2ALJ-4wGwm0mNLh5NFrKeONRmoOsOyuFomK90hs3lCmjUPzlius68Bol74rLjkrEadt8J4e8RNA)


### Step 3: Specify StackSet details, parameters, and deployment

- Enter the unqiue StackSet name.
- (Optional) Include a **StackSet description**
- **Parameters:** The following parameters need to be passed to the CF template


| ParameterKey | ParameterValue               | Modifiable?  |
| --- | --- | --- |
| AdditionalPolicy   | true                         | We highly recommend not modifying.This will ensure to create an additional policy for the Lambda Function Role giving access to KMS to attach the EBS volumes if the encryption is present in the Launch Configuration or Launch Template.  |
| AutoUpdate   | true                         | We highly recommend not modifying. AutoUpdate will ensure you have the latest version of Lambda in your account at all times without the need for manual updating.  |
| ClusterAutoScaler   | false                         | This must be set to 'true' only if you are configuring for Cluster AUtoscaler.  |
| Environment  | prd                          | This should not be modified, as this environment has been vigorously and thoroughly tested for stability.                                                           |
| ExcludeRegions        | comma-separated list of AWS region codes | If you don't have region based SCP's org. wide, leave it empty. The parameter 'ExcludeRegions' enables users to specify regions to be excluded from deployment by listing them, separated by commas.The value for this parameter can be blank or a comma-separated list of AWS region codes, for example, 'eu-west-2,eu-west-3'. If no value is provided during stack creation, it will default to an empty value, including all active regions by default.                                                                              |
| MemorySize   | 1024                         | This is the total memory for the Lambda Function. We recommend that you have this at 1024 MB for stable performance.                                                |
| ProjectID    | 0                            | NO                                                                                                                                                                  |
| Timeout      | 240                          | This is the Lambda function timeout in seconds. The timeout must be between 120 and 900 seconds                                                                     |
| Token        | Generated via nOps Dashboard | This is the token generated from the nOps Dashboard in **Step1**. It is not modifiable.                                                                             |



![](https://lh7-us.googleusercontent.com/gtDIG4dR3VomtdhF4QWIYMHlYqBylpdkrKpUKy77AV2mhrA1MnrzfxMiKZH_YeOdm1WIWCUiR7kkRltIYwxwfXgHE4rFGUtgLciGUBoVQQVJl9hXkMX72veyEysY6gIHgQMOut94aQTNp7tt3T7EMJ0)

- Click **Next**.
- In the **Configure StackSet options** add **Tags** if needed
- Leave the **Execution configuration** as **Inactive**
- Click **Next** In **Set deployment options**  Select **Deploy new Stacks**
- In **Deploy Targets** there will be 2 options 

1. Deploy to Organization
2. Deploy to Organizational Units (OUs)

![](https://lh7-us.googleusercontent.com/2eS6ZDNnsQp-ePdHnJUUvyL8-Hzz44zXrlmgePpBQc_C8sLPD7lq8LbJWGvqOUmpO3gDGMiwzXALiEwk7vw0ka4FmmomPWIrbhz9St27zMzPb0V-w2fkFRF_homgTD3Jkv_RmggsjfGYfkjuUBGH-CY)


### Step 4: (If deploying to Organization):

Select this option if you want to deploy the stack into all the child accounts of the organization. Upon completion, you will see a stack in all of the child accounts except the management (master-payer) account.


- Once you select **Deploy to Organization** do not modify the **Auto-deployment options**
- Under the **Specify regions** select either **us-east-1** (N.Virginia) or **us-west-2** (Oregon)
- Change the **Deployment options** to the following:

1. **Maximum concurrent accounts** (Optional): **change to percentage** > **100**
2. **Failure tolerance** (Optional): **change to percentage** > **20**
3. **Region concurrency** > **Parallel**

![](https://lh7-us.googleusercontent.com/GK94oSSM4ig5dI4MdrjXU17KzuT1Hp0Em7QW8iPbXWDfIgcGKttunqXNO3vNJRzUq34wCEWnS_z31JQ3RSY3Kz-vvuhK9Ewf5381IeYDNKU6nUDYFwkplOjCoG17UR4YSQ3yFjhKeF6NUntsYbeb7IY)

- Acknowledge and submit

![](https://lh7-us.googleusercontent.com/cWre6YezHI7fno_7W3uZnoJxbeIGP5Vx9RQtf4GJXghCaH_-o07xXqnIlsu0weAARUG3qo_vAIUIhzekZ_q7WZFV01LBGdjCyk1bv_OqhONGDwr29zsy9Lw9AI5XLKzAizNpcogwh8vUvgI70b6rkOk)

- Log in to any of the child accounts to verify the deployments

![](https://lh7-us.googleusercontent.com/Y3VDxKEObQ8HyaxjnQYtMc5tqRUrrnmIayjEN3lWj62pKHSR81yXlyM-uN9UftVdkPm_d9LqQDqiHb0hAXMsnwBpecze3I5OXzLsu_q0uT06ZrXLOgd2i-AjOjA15lm-In9rHkS4uB2Gi9nHpgd_ffY)

- Stackset creation successful

![](https://lh7-us.googleusercontent.com/a8jON88MPo88OhpINZ2l9YGY3t54K5psjQPK75IzASpoUkahGWxdB6J1nT3yCZ22wm7qeJRKlrCntouqVWNpZ5aqO11oR_xo7XUDbUsytXc4ZHo_AaDO2BFlCC7cwgaEqLjUZ8aH_sPnA0h98cg1Sfo)


### Step 4: (If deploying to Organizational Units):

Select this option to deploy the stack into a specific Organizational Unit of the Organization. Upon completion, you will see CloudFormation stacks in all the accounts that belong to that specific OU.

- Choose **Deploy to Organizational Units (OUs)** and add the **OU id**.

Note: Organization Unit IDs are located in **AWS Console > AWS Organizations**.
2.2 You can choose different accounts based on the **Account filter type** and add the account ids that you want to configure, separated by commas.
Note: Account IDs are located in the **AWS Console > AWS Organizations**.

![](https://lh7-us.googleusercontent.com/FjDOtUzE55N3zq8EBrIB0T6hL4ggrwfP6P_-nBhfW37GaxM2JLUBvFjvs4opeja5FqyiG8VUusNS3eQoZvFNFH7B2Kz8UBMCA59toyfALRARB1m7rU3VpMGuYffkm_co4hYleoz0hBgmrDIwnezV-Xg)

- Under **Specify regions** select either **us-east-1** (N.Virginia) or **us-west-2** (Oregon)
- Change the **Deployment options** to the following:

1. **Maximum concurrent accounts** (Optional):  **change to percentage** > **100**
2. **Failure tolerance** (Optional): **change to percentage** > **20**
3. **Region concurrency** > **Parallel**

![](https://lh7-us.googleusercontent.com/aIQY185v6ofnP4njSOT41hBH2UkwJHJip3gBaZPy1NFn0qUIdu6sabW7cvy1AJ4z5es21EbwQ12MdcRAF6hQOjR7frbt2cz3m6nCy0YGN66GsOR7XTwSZyWzRQq54zrB4eg9nvxhaECpuNVTO8G_LkY)

- Acknowledge and submit

![](https://lh7-us.googleusercontent.com/cWre6YezHI7fno_7W3uZnoJxbeIGP5Vx9RQtf4GJXghCaH_-o07xXqnIlsu0weAARUG3qo_vAIUIhzekZ_q7WZFV01LBGdjCyk1bv_OqhONGDwr29zsy9Lw9AI5XLKzAizNpcogwh8vUvgI70b6rkOk)

- Log in to any of the child accounts to verify the deployments

![](https://lh7-us.googleusercontent.com/Y3VDxKEObQ8HyaxjnQYtMc5tqRUrrnmIayjEN3lWj62pKHSR81yXlyM-uN9UftVdkPm_d9LqQDqiHb0hAXMsnwBpecze3I5OXzLsu_q0uT06ZrXLOgd2i-AjOjA15lm-In9rHkS4uB2Gi9nHpgd_ffY)

- Stackset creation successful 

![](https://lh7-us.googleusercontent.com/a8jON88MPo88OhpINZ2l9YGY3t54K5psjQPK75IzASpoUkahGWxdB6J1nT3yCZ22wm7qeJRKlrCntouqVWNpZ5aqO11oR_xo7XUDbUsytXc4ZHo_AaDO2BFlCC7cwgaEqLjUZ8aH_sPnA0h98cg1Sfo)


### Step 5: Verifying connection status on the nOps Dashboard

![](https://lh7-us.googleusercontent.com/6IrAKEPr9ogAAqd8NlD2YMVVFs8ajEoc-NcxK5ao_EILLWo44SPHuz1fSjwYkJ0zsrRcqwZyD6q_HcwST7jgKbEcUWuqfNTBgvrHzjWENwqWStWv2l1fY1dY_ZNeAe5g9qwOqYO3y3mPNtbpxvpwyNA)




## How to configure more accounts

Note: this guide will work only if you choose **Deploy to Organization Units** in the previous guide.

Let’s say you have already created AWS accounts in your Organization and want to onboard more AWS accounts. In this case, you don’t need to create a new StackSet. You just need to add more stacks to the existing one. This process is easier and faster than creating from scratch.


### Prerequisites

- StackSet configured
- Deployed to Organizational Units


### Step-by-step guide

- Log in to your **Master Payer AWS Console** with admin permissions.
- From within the **AWS Console > CloudFormation > Stacksets page**, click on the created StackSet.
- Click on **Actions** and **Add stacks to Stackset**.
- Choose **Deploy to Organizational Units (OUs)** and add **OU id**.
   Note: you can find Organization Unit IDs in **AWS Console > AWS Organizations**.

![](https://lh7-us.googleusercontent.com/AN3BUs5_3cw0bGtohGuKrcLIoJ2niS53KIACNdRtWSj7ge6GwnLfRrimsk-a0MwDg8f_CY36jaVCey2hvRcE5bL2tMQYR87d2YNfiwzA1Qd_1PUKTABXM8V4F49DOG8hLx7Lm3uWuWQSCYBJo8tZCWI)

- Choose **Intersection** in **Account filter type** and add the Account Ids (separated by commas) that you would like to configure.
   Note: you can find Account IDs in **AWS Console > AWS Organizations**.
- Under **Specify regions** select either **us-east-1** (N.Virginia) or **us-west-2** (Oregon)
- Please change the Deployment options as following:

a. Maximum concurrent accounts (Optional): change to percentage >- 100

b. Failure tolerance (Optional): change to percentage > 20

c. Region concurrency > Parallel

- Click on **Next**.

![](https://lh7-us.googleusercontent.com/Ugpq7HtSnexcA7r2auLqaAJnW18kZUAD20YgAwQnioX07tVc3toyUuqvkh1stOwukOiaYAj7CBdUrrxA6jXkuMlTZ_UkgnkVBoJSFfTPhn6OKsJpxj0__7S47EOR_9SBUiwyBZZ6Lkb6f5A54tlvVEk)

- Click **Submit**.
- After some time you will see the list of new **SUCCEEDED Stack Instances** for all new affected accounts. It means that all needed resources are created. 

![](https://lh7-us.googleusercontent.com/0LnA-MELsxbzJNXnpVLg8ZMD64r6tGPquFMRgljKEW6ji8uNlX_blko7xmQTfPKENsUmzKXol0BN6VrUcYN4F82vqIBBgt_t919nuilEm9cUV3mCMqfnu-zaG2DuKZi5BdCetTSFCASL4FR4QASeV6g)

- New affected accounts will have a **Connected** status.

![](https://lh7-us.googleusercontent.com/5RN8K4wVvYnjHszWZHzgI5Sf-o2-a_8n-lnXtDa-N7xl9xZwDEPWbpeVO1GBch-amwsO06XpzplFs16tVDziUi2hZv6E551ovXuy-5mcn1h0HGbS9PUKTdPFhcfNHWoKfBr0XcQmtDkIXlRm2kkm_CI)


## Resource Description

| Resource Name | Resource Type | Description  |
| --- | --- | --- |
| NASGEventBus                                  | EventBus                      | An event bus that receives events                                                                                                                                                                                                                 |
| NASGEventRuleEC2InstanceStateChange           | Events Rule                   | An event rule that monitors the change in the state of EC2 Instances (triggered on a new ASG instance launch)                                                                                                                                     |
| NASGEventRuleEC2InstanceStateChangePermission | Lambda Permission             | Grants _events.amazonaws.com_ permissions to invoke the _NASGFunction_ lambda function based on the _NASGEventRuleEC2InstanceStateChange_ Events Rule                                                                                             |
| NASGEventRuleScheduledCheck                   | Events Rule                   | An event rule that triggers the _NASGFunction_ Lambda function every 30 minutes to analyze configured ASGs                                                                                                                                        |
| NASGEventRuleScheduledCheck Permission        | Lambda Permission             | Grants _events.amazonaws.com_ permissions to invoke the _NASGFunction_ Lambda function based on the _NASGEventRuleScheduledCheck_ Events Rule                                                                                                     |
| NASGFunction                                  | Lambda Function               | A Lambda function to handle events from _NASGEventBus._ This lambda does all the work related to ASG instance replacement.                                                                                                                        |
| NASGFunctionRole                              | IAM Role                      | An IAM Role that’s necessary for the Lambda function “NASGFunction” and “NASGLambdaSelfTestFunction” with all the necessary actions related to autoscaling and EC2                                                                                |
| NASGLambdaSelfTestFunction                    | Lambda Function               | A Lambda function that verifies the  _NASGFunction_ Lambda function on stack deployment and reports its status to nOps                                                                                                                            |
| NASGEventBridgeForwarderMultiRegion           | Stackset                      | An EventBridge forwarder that gets created via stack instances of the stackset in all the SCP-allowed regions of the same AWS account. So the events from all configured regions are forwarded to the lambda main region (us-east-1 or us-west-2) |
| NASGRoleCheckerRole                           | IAM Role                      | An IAM Role used for the Lambda function “NASGRoleCheckerFunction” which has policy to read, create or modify IAM roles                                                                                                                           |
| NASGRoleCheckerFunction                       | Lambda Function               | A Lambda function that verifies the existence of IAM Roles in the AWS account. These roles enable us to create a stackset in the same account which will deploy an EventBridge forwarder in all the SCP enabled regions.                          |
| RoleChecker                                   | CloudFormation CustomResource | A Custom Resource sends a request to _NASGRoleCheckerFunction_ and waits for a response before proceeding with the stack operation.                                                                                                               |
| PrimerInvoke                                  | CloudFormation CustomResource | A Custom Resource sends a request to _NASGFunction_ and waits for a response before proceeding with the stack operation.                                                                                                                          |
| nOpsCrossAccountRole                          | IAM Role                      | A cross account role, which has a trust relationship with the nOps account for the Auto update feature to assume role into. It has the “nOpsCrossAccountPolicy” attached. It is going to be created only if you set AutoUpdate parameter to true. |
| nOpsCrossAccountPolicy                        | IAM Policy                    | The policy which is attached to “nOpsCrossAccountRole” and has permissions only for the stack & stack resources. These are read-only and update permissions.                                                                                      |





{% include custom/series_related.html %}
