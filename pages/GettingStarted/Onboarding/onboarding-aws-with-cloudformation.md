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

nOps requires safe, secure, and AWS-approved access to your AWS accounts in order to give you the analysis, dashboards, and reports that you need. We only see what you want us to see in order to provide our services, no more, and we need you to give us permission first.

In order to credential and register multiple accounts, we leverage _AWS Organizations_, _CloudFormation_, _Stack_, _StackSets_, and _Lambda_.

For multi-account setup, nOps recommends the use of CloudFormation (this setup) instead of Terraform (intended for advanced users with specific requirements).

**Watch the video on how to configure linked accounts with CloudFormation:** 

{% include video.html content="https://nops-docs-vids.s3.amazonaws.com/nOps-cloud-formation-stackset.mp4" vidclass="vid800600" vidwidth="100%" vidheight="100%" %}




## Prerequisites ##

*   You must have Admin role permissions in AWS before you can add multiple AWS accounts to nOps using _CloudFormation_.
    
*   Access to the nOps public Github repository [nOps Cloud Account Registration](https://github.com/nops-io/nops-cloud-account-registration).

*   You have [configured your Payer account](onboarding-aws-with-automatic-setup).

*   Enable _Stackset_ in _AWS Organizations_ and _AWS CloudFormation_ within AWS.
    

Once you’ve taken care of the prerequisites, the next steps are simple and straightforward.

## Adding Multiple AWS Accounts (CloudFormation) ##

Pull the [nOps Member Account Registration YAML](https://github.com/nops-io/nops-cloud-account-registration/blob/main/nops-aws-account-register/cloudformation-org-member-accounts-register/member_consolidated_aws_acc_nops_register.yaml) file down as a local YAML file. You will need this CloudFormation YAML file as a template for your StackSet.  You will also need the nOps API key.

### Generate your API key ###

To generate your API key for use with CloudFormation Stacksets, log into the nOps platform.

1. Click on your email address to the top right of the platform
1. Navigate to **Organization Settings** > **API Key**
1. Click "Let's Generate Your API Key"
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-org-api-key-menu.png)
1. Enter a key name and a description.
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-org-api-key-name-description.png)
1. When you click **Save** a pop-up box will display with a 1 time key. Copy the key to a notepad/text editor.
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-api-key-generated.png)

## Enable Stacksets ##

To enable _CloudFormation StackSets_ in _AWS Organizations_, go to **AWS Organizations > Services**. If you see **Access disabled** for _CloudFormation StackSets_, you will need to enable it.

Once enabled, you should see **Access enabled**:

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-aws-cloudformation-enabled.png)

Also ensure Trusted Access is enabled for **CloudFormation** > **StackSets**.


## Create a Stackset for the Linked Accounts ##

CloudFormation Stacksets can be multi-account and multi-regional. To create and deploy a stackset for the linked accounts, make sure that you are logged into your Management Account.

From within **AWS Console > CloudFormation > Stacksets** page, click **Create Stackset**. 

The creation of a Stackset is divided into 5 steps:

### **Step 1 (Choose a template)** ###

1.  In the **Specify template** section, choose **Upload a template file** option.
    
2.  Click **Choose file**.
    
3.  AWS will open a navigation window for you to navigate and select the YAML template in your local machine. In your local copy of the repository navigate to **nops-cloud-account-registration/nops-aws-account-register/cloudformation-org-member-accounts-register/** and select the **_member\_consolidated\_aws\_acc\_nops\_register.yaml_** file.

    ![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-aws-stacksets-uploaded.png)
    
4.  Click **Next**.
    

### **Step 2 (Specify Stackset details)** ###

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-aws-stackset-details.png)

1.  Provide a **StackSet name**.
    
2.  (Optional) Add a **Description** for the StackSet.
    
3.  Provide the **nOpsAPIKey** you copied earlier.
    
4.  Click **Next**.
    

### **Step 3 (Configure Stackset options)** ###

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/gs-aws-stacksets-execution-config.png)

1.  (Optional) enter any tags for the StackSet.

2.  In the **Execution configuration** section, leave the **Inactive** option selected.
    
3.  Click **Next**.
    

### **Step 4 (Set deployment options)** ###

1.  In the **Add stacks to stack set** section, select the **Deploy new stacks** option.
    
2.  In the **Deploy targets** section, select the **Deploy stacks in organizational units** option.
    
3.  Provide the organizational unit ID.
    
4.  In the **Specify regions sectio**n, select your desired region.
    
5.  In the **Deployment options** section, select the **Parallel** option (optional).
    
6.  Click **Next**
    

### **Step 5 (Review), review and create the stackset.** ###

## Fetching ##

It might take several hours for nOps to fetch the data from your AWS accounts.

After the data is fetched, the setup process is now complete.

Note: It can take up to 24 hours before you start seeing the different nOps dashboards and compliance views populated with data from your workloads.

If you have any questions, please contact us at [help@nops.io](mailto:help@nops.io).

On initial ingestion, nOps will pull the data from AWS accounts based on the following durations:

*   Cost data: 6 months look back + current month.
    
*   Rules: Current date.
    
*   CloudTrail Events: 14 day look back.


{% include custom/series_related.html %}