---
title: Onboarding your Autoscaling Groups to nOps Compute Copilot
keywords: savings, recommendations, sharesave, asg, copilot, autoscaling
tags: [copilot, asg]
sidebar: mydoc_sidebar
permalink: copilot-asg-onboarding.html
folder: Copilot
series: [Copilot, Onboarding]
weight: 1.0
---


# Introduction to Compute Copilot for Autoscaling Groups (ASG) #

## About Compute Copilot for ASG and how it works ##


- Compute Copilot is a service that automatically optimizes any compute-based workload. It leverages ML algorithms to reduce your AWS ASG costs by automatically migrating EC2 instances to Spot. 
- Compute Copilot Lambda is aware whenever ASG launches a new on-demand instance (such as in response to a desired capacity change). In response, it automatically launches a Spot instance with settings that mirror those of the on-demand instance, attaches it to the ASG, confirms it is in service, and removes the on-demand instance for you.


## Why Compute Copilot ##

- Compute Copilot uses AI-driven decision making to provision and run auto scaling group instances at the cheapest price in real time, without manual effort.
- Compute Copilot allows you to benefit from Spot savings with the same reliability as on-demand. By analyzing historical data and Spot Termination events, it ensures your critical workloads remain safe from interruption. 
- Compute Copilot does not require your workload to be transferred to a proprietary system, but works directly with AWS ASG.
- Compute Copilot does not typically modify the ASG settings, allowing it to be disabled at any time without interrupting existing workflows.

## Prerequisites ##

1. You must be logged in to your [nOps account](https://app.nops.io/accounts/signin/). 
2. Your AWS account must be configured to your nOps account.
3. You must have the correct permissions. 

In the Compute Copilot Auto Scaling Groups Onboarding call, we will launch a CloudFormation Stack to create (1) a Lambda and its associated resources, and (2) a StackSet to create an EventBridge Forwarder in the same account but in all the SCP enabled regions of the same account.



## Steps to Configure Your ASG Cluster ##



### Generate API Key ###
- Navigate to Organization Settings → API key.
- Generate a New API Key, and save it to use later in the ASG Lambda CloudFormation stack
    ![](https://lh7-us.googleusercontent.com/DUWPDI1CSnO5kyjylFhvv0hzlluqiRjkz6vWk-1lT6_xhsgReVNxdFxNTd2j2hmaYeiXUE3uz4mQVl5GKA_MakQuTMfMIBdPALiP4tWhSC5qLhLsYKCpE70RaeOat6t6FKENiNkVs0eQNKO6TYkZzaQ)

    **_Note: you can use one key for multiple AWS accounts or stacks._**



### Launch ASG Lambda Stack in the AWS Account ###
1. In the nOps dashboard, Navigate to **Organization Settings** → **Integrations** → **ASG Lambda**.
- Select the AWS account where you want to run the ASG Lambda Stack.
- Select **Launch ASG Lambda Stack**. You will be redirected to the Cloud formation stack in the AWS console.
- Input the API key that you created in step 1.2 into the Token field.
- Acknowledge the capabilities and create the stack.
- Please verify in your AWS account that you are able to see the following StackSet and stack that was created via Terraform.
    ![](https://lh7-us.googleusercontent.com/gYsBQgyqah3mLZA3s288f9x1UrpoJ9bF4dbswlfMlCH1IEMWvExs4YEndnlGUYGvCfPEozQii1Umw0FSwirGd7TlRh6k7dGsYeZI3ASlK4HaZ7h_KjSCOp3UXiinqI0-bHrB6HRUZ2p1zVq01704g58)
        
    _Under Stacks, you should be able to see the above in Stack instances in the same region as stacks._
    ![](https://lh7-us.googleusercontent.com/lwMnfBrpJZR_blrNdWoHK9NMkDguDxq1RElS80U5SUBxYRgWf26O6761sBvVdTGBZZUfUd86hOEXPI5boVZ8lYAQqQIPn18A7H7xl0wIPRvmOJmMx-SVJacikpb5__EM4EA4KVB_HteL6oHLsCbJX-c)\
    Notes: Within StackSets, the above should be visible in the StackSet within the same region as the stacks. Please confirm the status. It may be observed that the StackSet is creating stacks in only a few regions, rather than all intended regions. This is not of concern, as the failure to create stacks in other regions could be due to Service Control Policies (SCPs).

2. After successful completion, return to the nOps platform and refresh the ASG Lambda page.
3. You should see the updated version of Lambda, with the status showing a successful connection to the configured AWS account.

    ![](https://lh7-us.googleusercontent.com/_ybOWytEHm4GzM5u7JQEXpHCj05GsKTAJkPU21YddvDzJhZ5eFlRwaRmSOuZbFTgGSPw4udDuKKVFt7C9lyAy_U8bQQA7RVeE956p8pL7-EAzEfUAjXLe5CFSbANW25A75_UCWvmz1FwUU00Aye16DI)
    
    **Note:** 
    1. Before the ASG Lambda stack runs,
    - The status of the version shows N/A by default.
    - Auto Update is enabled by default, allowing Compute Copilot to automatically update to the newest version of Lambda. This setting can be disabled by the user if desired.
    2. **Redeploy button for lambda:** After deleting a stack from your AWS account, it may take up to 30 minutes for the status to update on the nOps platform. To expedite this process, we've introduced a redeploy button 'Redeploy ASG Lambda Stack' enabling you to swiftly relaunch the ASG Lambda stack.


### Select the ASG cluster and configure ###

1. Navigate to the ASG Dashboard and select the ASG cluster to configure
    - Open Configure Model. The details section of Auto Scaling Group will be prefilled. 
    - The AWS Lambda configuration section will show the version detail and current status of the Lambda Stack to confirm it is properly configured on the AWS account. 
    - You can create or choose an existing ASG template in the Spot detail section.

2. Create an ASG Template.
    - Give the ASG template a unique name
    - Select the CPU architecture based on the AMIs of the ASG you are going to attach the template to
    - By default **Dynamic min VCpuCount & MemoryMiB** is checked, setting the minimum vCPU and RAM requirements based on the size of the On-Demand EC2 instance being replaced.You can disable this option and set the CPU or Memory suitable for your workload from the **Instance Requirements** list.
    - (Optional) When selecting instance requirements, eligible instance families will be highlighted based on your criteria. To simplify the selection process, you can choose all highlighted options by clicking on the "Select All Eligible Instance Families" link. 
    - You can also directly choose instance families. Compute Copilot will select the most optimal choice for price and stability out of the provided options.
    - Now select **Create**

    ![](https://lh7-us.googleusercontent.com/Q9GKw25eU6EurrlMYzWLmSFVGyZ7TsNHeshn11McNoGkgd1U2xL0GRc58OUueykpQdarSPhPBIoFqdBngCeNLDuopSM4LXqaHSKC09FiMr8zohEeot1RoHV4EBlvRHj0EPgQvNJeMAmToVS9Vl9qwHA)
    
    _Note: you can also clone an existing template to fetch pre-defined configurations to apply to a new template._ 

3. Select **Fully Managed ASG** if you want your ASG to be [fully managed by Compute Copilot Lambda](https://help.nops.io/copilot-managed-asg.html), utilizing the latest features like Instance Refresh and Mixed Instance Policy instead of it making the OnDemand to Spot replacements after the instance launch. 

4. **Ignore Scale-In protection** - On selecting this option, Compute Copilot ASG Lambda will ignore Scale-in Protection on your instances and will replace them. By Default this is not selected.

5. Set the **Minimum Number of On-demand Instances**
**Minimum Number of On-Demand Instances** defines the number of On-Demand instances that should be left in the ASG and not replaced with Spot. ASG Lambda won’t do any On-Demand to Spot replacement if it has fewerf On-Demands instances than specified in this config setting.

6. Set the **Spot percentage** and **Max Spot Instances** using the draggable bars.

**Spot percentage** defines the percentage of on-demand instances to be replaced with Spot. 

**Max Spot Instances** defines the maximum of Spot instances to be created by Compute Copilot.


1. Once values are defined, select Configure.  

2. ASG status will now display **Configured.**
    ![](https://lh7-us.googleusercontent.com/APdoXTA6S1LPpOvcrHLVg8kAZkpbJ-4Dbdmf-behnSC2Vk7J6XDvCICgIzMXEae1MmCh1KwvsgIbp7Qy4OgH-M-XNosbHqLRaJTHVeG8_GNTWNMzcSOGwlAFXfAdbYGrAH8w2hgUjfgt2_FAoPn7r9w)


### What to expect after configuring ASG

Compute Copilot Lambda will begin replacing on-demand instances in this ASG with Spot alternatives, either every 30 minutes or upon the launch of a new on-demand instance.

**History of Actions**
After configuring your ASG, Lambda starts replacing On-Demand instances with Spot instances, either every 30 minutes or on the launch of a new On-Demand instance.To capture the replacement in logs there is “History” button on the ASG-dashboard beside the Configured ASGs which shows the “History of Actions” for the ASGs. 

If there are any logs about the replacements in the past 7 days then the History button will be in Green; otherwise, it is disabled.

To check the “History of Actions” click on **History** and you can see the list of all the actions that have taken place with the date and time.The system also identifies issues with Auto Scaling Groups (ASGs), such as when they are associated with an incorrect (non-existent) Launch Template specified in the tags. ASGs with such errors will be marked with an error icon and their background color set to red in the list. \
![](https://lh7-us.googleusercontent.com/Vs9kRrsWbXfqUEmCBGpSbZ7ndN5RtUJZ42cZ3nvQWagjswCf3ksDJWmWOm1INJzHtBe4ZpX_DD-sEqKW0Uw0RnYDbbuw4t9jaDw-f19Mw4xtffejF_TTyNOJWDu50_hFpSBhL8UJn-rviRxEOq985Zo)

### Tagging Structure of Spot Instances Launched by nOps

When Compute Copilot launches a Spot EC2 instance to replace an On-Demand EC2 instance in an ASG, it copies all the tags attached to the On-Demand instance being replaced, except for AWS reserved tags (prefixed with aws:).

Additionally, it adds three new tags to the EC2 Spot instance:

| **Tag Key**                        | **Description**                                                            |
|------------------------------------|----------------------------------------------------------------------------|
| launched\_by\_nops\_asg            | Tag to track that this instance was launched by ASG Compute Copilot        |
| launched\_for\_asg                 | Tag to track the ASG name for which this instance is launched              |
| launched\_for\_replacing\_instance | Tag to track the On-Demand EC2 instance ID that this instance is replacing |


## FAQ



1. **How does Compute Copilot replace on-demand instances?**
    - For the nOps Copilot-Managed ASGs, the process of replacing on-demand instances with spot instances is described in the [nOps Copilot-Managed ASGs](https://help.nops.io/copilot-managed-asg.html) section.
    
    - If the ASG is configured using the original approach, the following steps take place:
    ![](https://lh7-us.googleusercontent.com/OjZwU_zk5KM1CXUihO_evtZwp-L6bkNXBvQHzcqCdD39OkO-t_KdEcQLYKBKLYV1l8SO2GLJgB7jtaG6Rr_9gWX-FpbznWEPW1Xt7Sv9ClZX0wJW6-YhrMTHgStuHXWM43z_H0-pZsAKVHRxXNSpzsM)
    
    1. ASG launches a new on-demand instance
    2. Lambda intercepts the EC2 Instance State-change Notification event from EventBridge
    3. If the created instance is not protected from termination and should be replaced, Compute Copilot performs the following steps. 
        1. Copy the Launch Template or Launch Configuration from the ASG launch template
        2. In the copied Launch Template, modify Network Interfaces, Tags, Block Device and/or Mappings from the instance Launch Template or Configuration if needed
        3. Fetch recommended instance types from nOps API
        4. Request Spot Fleet with the copied Launch Template and recommended instance types
        5. Once the Spot request is fulfilled, get the Spot Instance and wait for it to its state to be Running
        6. Attach the created Spot Instance to the ASG
        7. Wait for the attached Spot Instance’s state to be InService
        8. Terminate the on-demand instance

2. **Why don’t I see some of the instance families when I create a Launch Template?**

    Instance families that are not compliant with the ASG you are attempting to configure are automatically hidden. For example, if your ASG uses instances with arm64 CPU architecture, other instance families that are x86\_64 will be filtered out from the list.

3. **What is Auto Update Lambda, and how does it work?** 

    When Auto Update is enabled, your Lambda will be automatically updated every time a new Compute Copilot version is released (12 p.m. UTC). When you deploy Compute Copilot Lambda with Auto Update, the stack will also contain a Cross-Account role.  


4. **What permissions does Auto-Update Cross-Account policy have?**


```yaml 

Action:
- cloudformation:CreateChangeSet
- cloudformation:DescribeStackInstance
- cloudformation:DescribeStackResources
- cloudformation:DescribeStacks
- cloudformation:GetTemplateSummary
- cloudformation:UpdateStack
Resource:
- !Ref AWS::StackId

Action:
- cloudformation:CreateChangeSet
- cloudformation:DescribeStackEvents
- cloudformation:DescribeStackInstance
- cloudformation:DescribeStackResources
- cloudformation:DescribeStacks
- cloudformation:DescribeStackSet
- cloudformation:DescribeStackSetOperation
- cloudformation:GetTemplateSummary
- cloudformation:UpdateStackInstances
- cloudformation:UpdateStackSet
Resource:
- !Sub NASGEventBridgeForwarderMultiRegion

```

5. **Will instances protected by ASG `Scale-in Protection` or EC2 `Termination Protection` be terminated?**
    
    No, instances protected with ASG `Scale-in Protection` or EC2 `Termination Protection` will not be terminated. All instances protected by these settings within configured ASGs will not be replaced or terminated by Compute Copilot.



{% include custom/series_related.html %}