---
title: Maximizing Cost Efficiency with nOps Essentials Idle Instance Feature
keywords: savings, recommendations, sharesave, essentials
tags: [essentials]
sidebar: mydoc_sidebar
permalink: Maximizing-Cost-Efficiency-with-nOps-Essentials-Idle-Instance-Feature.html
folder: Essentials
series: [Essentials, Storage]
weight: 2.0
---


### Maximizing Cost Savings on your Cloud Spend with nOps Essentials Idle Instance Feature<a id="maximizing-cost-savings-on-your-cloud-spend-with-nops-essentials-idle-instance-feature"></a>

In the realm of AWS management, the accumulation of unused EC2 instances can lead to unnecessary costs for businesses. Whether remnants of past migrations, misconfigurations, or abandoned projects, these idle instances draining resources without adding value. However, nOps Essentials is introducing a game-changing feature to tackle this issue head-on. Now, users can halt idle EC2 instances with a single click, allowing you to instantly reduce costs while retaining operational flexibility to roll back if needed. Stopping idle instances is one of the most effective cloud optimization strategies — for every dollar spent on an instance, you save two dollars in corollary charges such as storage, network and database fees.

**What is an Idle Instance?**

An idle instance, as defined by nOps, is an EC2 instance exhibiting less than 1% CPU utilization over a period of 30 days. This definition specifically excludes any instances within Auto Scaling Groups (ASGs), Amazon Elastic Kubernetes Service (EKS) clusters, and instances with ephemeral storage. 

**How does this feature work?**

Here's a detailed breakdown of how the feature operates:

1. **CloudWatch Integration:** nOps integrates with your AWS-native CloudWatch to collect usage data. It continuously monitors relevant resource-level metrics such as CPU to find idle instances.
<br />
2. **Identification of Idle Instances:** Through advanced algorithms and analysis, nOps sifts through the collected data to identify instances exhibiting consistently low CPU utilization over a specified period. These instances are flagged as idle and earmarked for optimization.
<br />
3. **EventBridge and Lambda Functions:** nOps uses EventBridge and Lambda to automate tasks by triggering serverless functions based on specific events, facilitating seamless communication with AWS services.
<br />
4. **Initiating the Stopping Process:** Once idle instances are identified, nOps utilizes EventBridge and Lambda functions to trigger the process of pausing them. This involves sending commands to the AWS infrastructure to stop the identified instances, preventing them from consuming resources unnecessarily while retaining the flexibility to roll back if needed

**Is this automation reversible?**

Users can reverse the automation if needed by restarting EC2 instances through the AWS Management Console or AWS Command Line Interface (CLI). It is crucial to exercise caution to ensure the continuity of operations, particularly regarding associated resources like volumes and Elastic IPs. nOps Essentials' Idle Instance Cleanup feature is designed to leave associated resources untouched, preserving operational continuity and minimizing any potential disruptions.

**Step-by-Step Guide on How to Stop Idle Instances with nOps Essentials**

1. **Sign In**: Sign in to the [nOps platform](https://app.nops.io/accounts/signin?next=/landing/) using your credentials.

2. **Access Idle Instance Recommendations:** Navigate to the Essentials Menu and select the [Idle Instance Page](https://uat2.nops.io/v3/essentials/idle-resources/). Here, you'll find recommendations for idle instances to stop.

3. **Review Recommendations:** Take a moment to review your recommendations.  You can also view details such as Resources Details and its usage Chart, Environment, Resource Count, Yearly Savings, and an action button to view more details and create the automation.

![](https://lh7-us.googleusercontent.com/y321ceMULow29EdnGZ5gIFKfPE0FqV8LpbBc3cXTvLfwLsCxqeYp9gz_RSNTTCJvxtHglzCUx7T9jZgUn_LSgkddmBpCpRHGNlSDJRYc5TrQ5H6RSDuoIp39zINOCgg2dnYpvA4_Y15QGXVsiOugGlc)


![](https://lh7-us.googleusercontent.com/Q3P6jPSHxWq1NCAMLo6zOiIoXqPRLZcHFiSoMrAKgCerrYHP574MknBp6gH3gq4gOqlg3SL4AOaZ49pXNYyrFlp8WGRuYgO90kI-NFCBsBZmmvRz85PTY-YCDqLmI45NYHePOF33pwJ4gXMgBqGJzx8)
 
![](https://lh7-us.googleusercontent.com/lfArgTfrMYeIRiVr_jUG-4cD5pjPndB6-g5BJIY6lnE5FSIuREbMDYkFLaHjH78mqHXA76q43_2XoFckoL-HHV7TIZxBiRaF71DwuQK2wnsg9JPqmWRRho3ef8hivPw71iYSTvxTPGDHeDF8ZP9S3lM)

4. **On Action Page:** Here, you'll need to either select EventBridge or configure it if it's not available. This step is crucial for automation. Additionally, choose the desired time frame for the automation to run, ensuring it aligns with your operational needs.

![](https://lh7-us.googleusercontent.com/KSLb-qQLVxlLB5tRKgrDnIeT1muTWzJ_mpYWeqDlwdKbtknQ7Wj6mF0QD17aOvDKLyxRtdkEsUGjB3HaRnwRGhdkWJFehS8Fs-wR9H7f-DpEY1072rF3CrQSm5jrwsW5RJZgqG-UMUsygl46U_hIZPU)

5. **Confirm Selection and Estimated Savings**: By default, all EC2 instances are selected. You can deselect any instances if needed. Review the estimated savings and confirm the action.

![](https://lh7-us.googleusercontent.com/ld2BdDcmz2N_WH9ew1i4kJOTcakldvScJH1k9rfqi7pXp99xvqfMhKI-Eruz21fSUDvUY7x5rPAwjA7-_QSNCHtFOxOUBJwdayTHt2dv5-kGipvFgjCHmcVrIAe4XhJfDodeZmayhGMK7BPndqdkEJY)

6. **Initiate Stopping Process:** Click on the "Stop Instances" button to initiate the process. You'll be redirected to the Resource nSwitch Tab, where you can Verify Automation Status and see the automation schedule. If you've selected "Now" for automation, by default, it will be scheduled 10 minutes from now. You have 10 minutes to cancel the automation by deleting the schedule. Once the automation triggers, all selected instances will be stopped accordingly.

![](https://lh7-us.googleusercontent.com/Wl3XW3_ObVPhYih1vy1A5WQQbZS7_ojJGG6BoVRzAJvlDpgBvPHdCdVdR5Ud85XU4lSukp7iAR46nT0zMOWobHsB4xkjJ_FjJJ2t9LC8Bz_ek9Vck6YxU1gXzYhNe_p3QP0J9Ij1mw2h2rBfwifcq0U)

7. **Check Resource Status:** Upon scheduled execution, check the status of each individual resource in the Resource popup. This ensures that the stopping process has been successfully executed for each selected instance.

**Conclusion**

With nOps Essentials' Idle Instance feature, businesses can proactively manage their AWS costs by identifying and halting idle EC2 instances. By streamlining this process into a single click, nOps empowers users to optimize their cloud expenditure without compromising operational efficiency.


<br/><br/>

{% include custom/series_related.html %}