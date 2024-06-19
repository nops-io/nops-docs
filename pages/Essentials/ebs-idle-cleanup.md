---
title: Idle EBS Volume Cleanup
keywords: savings, recommendations, cloudwatch, essentials, ebs, idle ebs, waste cleanup, cleanups
tags: [essentials]
sidebar: mydoc_sidebar
permalink: ebs-idle-cleanup.html
folder: Essentials
series: [Essentials, Rightsizing]
weight: 2.0
---


# Reduce your cloud cost by cleaning up your Idle EBS Volumes<a id="reduce-your-cost-by-cleaning-your-idle-ebs-volumes"></a>

When EC2 instances are frequently launched and terminated, orphaned EBS volumes often go unnoticed, leading to significant cloud waste. Unless the “Delete on Termination” option is selected during the instance launch, terminating an EC2 instance will only detach the associated EBS volume rather than delete it. Consequently, unused EBS volumes can accumulate over weeks or months, consuming resources and increasing costs.

## What is an Idle EBS Volume? ##

An idle EBS volume, defined by nOps,  is an EBS volume that has been orphaned or unattached from any EC2 instance for a period exceeding 30 days. These volumes can accumulate unnoticed, consuming storage resources and contributing to unnecessary costs. Identifying and managing idle EBS volumes is crucial for optimizing cloud resource utilization and minimizing expenses.

Here’s how nOps enhances the process:

- **Effortless EBS Volume Cleanup**: nOps simplifies the identification and deletion of unused EBS volumes with a single click, eliminating the need for manual cleanup and freeing up valuable engineering resources.

- **Minimal Risk**: Before deleting a volume, nOps provides the option to take an economical snapshot, ensuring you have a backup available if a rollback is needed. This feature offers peace of mind while optimizing costs.

- **Intelligent Savings**: nOps delivers recommendations that guarantee net savings. For instance, it avoids suggesting snapshot creation for Cold HDD (sc1) volumes, where savings might not be realized, ensuring that every action taken results in tangible cost benefits.


## How it Works<a id="how-it-works"></a> ##

1. If you’re onboarded to nOps, data will automatically be collected from your AWS API.

2.  If an EBS volume is not associated or attached with any EC2 instance for the last 30 days, it is considered orphaned.

3. You have two options when cleaning up EBS volumes: you can either create a snapshot before deleting the volume to ensure a backup is available, or you can choose to delete the volume immediately without creating a snapshot for a quicker cleanup and to avoid paying the small fee of storing the snapshot.

## Step-by-Step Guide on How to Stop Idle Instances with nOps Essentials ##

1. **Sign In**: Sign in to the [nOps platform](https://app.nops.io/accounts/signin?next=/landing/) using your credentials.

2. **Access Idle EBS Volumes Recommendations:** Navigate to the Essentials Menu and select the [Idle Instance Page](https://app.nops.io/v3/essentials/idle-resources/). Here, you’ll find recommendations for idle EBS Volumes to stop.

3. **Review Recommendations:** Take a moment to review your recommendations.  You can view details such as Environment, Resource Count, Yearly Savings, and an action button to view more details and create the automation.![](https://lh7-us.googleusercontent.com/-GPabPErBDbjAxu4_HUlmAPXJAoaqpwZBqqPPVF-U_deyOGQ64q2ab5JUY1yePw2zW-eLEaqSNMNWo52f-LKLqqvjaVPNCxBROJkjDUxJiYPSdwhEwJSGNu74EL13FLb5k6-nVR3QAJGtgi4ZEB94VA)********

4. **On Action Page:** Here, you’ll need to either select EventBridge or [configure it if it’s not available](https://help.nops.io/essentials-stack.html). This step is crucial for automation. Additionally, choose the desired time frame for the automation to run, ensuring it aligns with your operational needs.![](https://lh7-us.googleusercontent.com/8iOqSpzcxxMezAyTPnznNBX6K8O4eJIowJOqwOJ5KTBdHxk1eVHePUYw2IzLvjjLnYG_au-IontfaDPu0QaYn32rgRqLh19a9W6X-ii2wuTuuI4YXiMqXqn1XFuFcEIlPdCK16Ky2j6tbQm-cJSQNFE)

5) **Confirm Selection and Estimated Savings**: By default, all EBS volumes are selected. You can deselect any instances if needed. Review the estimated savings and confirm the action.

6. **Confirm Automation**: When cleaning up EBS volumes, you can choose whether to create a snapshot before deleting the volume. 

   - If you want to preserve the option to roll back, you can click the “Snapshot & Cleanup” button to automatically create snapshots of all the EBS volumes that you have selected in the recommendations. 

![](https://lh7-us.googleusercontent.com/ezYZDUNWWSTEkFi2UboopDrhWboFNcwnQksOSP7JIi8A-WRUujE0nJvpCLtJymS2lvla0-8kfaRxtBBPzKwB_HAJzHbapDw8YNl4MEoPyJMdLedXAjGp00bDuBbSSjSH3QndI0xb7KCIQ8iExU-oluM)

- If you want to delete the EBS volume without a snapshot, you can do so by clicking the “Cleanup” button to confirm the automation.

![](https://lh7-us.googleusercontent.com/Y80x0xG46l5yMc8RxQbXSU9ncO8BTzzwH9nTutJ_vOx_AmdDACTj9nQtYIIKG_XMJRGAWDwE9oKv8tALJDoE-5Qpe0T4OWhhIaEXXVmw_-_SVcGHy6zxjK0LM8F3s12Jgh2fsd96-OcliWPkDPPigVg)

7. **Initiate Stopping Process:** Once you confirm the cleanup, you will be redirected to the Summary tab of the Idle Resources page, where you can Verify Automation Status and see the automation schedule. If you’ve selected “Now” for automation, by default, it will be scheduled 10 minutes from now. You have 10 minutes to cancel the automation by deleting the schedule. Once the automation triggers, all selected volumes will be deleted accordingly.

![](https://lh7-us.googleusercontent.com/aQJ9tw3y_bkY9stOFx7e7iclVy5W7CFDu6jOUQuGqaT6rxsSD5NbQXksiWRjVklFMtMNjCcKxHtzAQwCcFE1e837HyemYgCUrIy6h7va8faZuPu3FFUU8S9OeRN_eoa3NRIWdxiBkbauoSwlnhT3V24)



<br/><br/>

{% include custom/series_related.html %}