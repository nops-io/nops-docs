---
title: Introduction to nOps Essentials
keywords: scheduler, storage, rightsizing, idle_resources, essentials, eventbridge, stessentials, optimization
tags: [essentials, scheduler, storage, rightsizing, idle_resources]
sidebar: mydoc_sidebar
permalink: essentials-introduction.html
folder: Essentials
series: [Essentials]
weight: 1.0
---


# Introduction to nOps Essentials #

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXf4sHNIo0PDJVJUyYNWYIPw4AOXaiOe3EHjrXfT-BRymO5e0imsbuPFgJidDsuCeroLTJRDN0jcDRDwGmuyaDmOf6GigUe4gif3TnvhalkR9IkZlwPmxVbOCT7GZNabIubc4odbfk-MEHnLxs6A3qq8xZDT?key=ZakVLgDvbyBcOtmV15U4Kw)

The menu provides an overview of various nOps Cloud Optimization Essentials features to help streamline and automate your cloud resource management and cost efficiency. These include:

1. **Essentials Summary**

   - Overview of all savings opportunities and realizations for nOps Essentials 

2. **Rightsizing**

   - Right sizing EC2 instances and Auto Scaling Groups

3. **Scheduler**

   - Automates AWS resource scheduling to reduce costs during off-hours

4. **Idle Resources**

   - Finds and stops unused resources such as EC2 and EBS volumes 

5. **Storage**

   - Automatic migration of EBS volumes from GP2 to GP3 via EventBridge or Storage Changeset.

   - EBS Snapshot optimization


## Essentials Summary ##
[Essentials Summary](https://help.nops.io/essentials-summary-page.html)


![](https://lh7-us.googleusercontent.com/docsz/AD_4nXe90o8B_tLZItw_5cbNMDXUCGOFI56RGOyJ5WOb5KWYIUObErCkBIBfqqeiH0NAljxcauptOi4bv2MLysH9d21I4NCAxrXyf3T8zcFi6Eo_2I09jGU0p3JqSB6tN-2UDwJdHroYdmYfkMILH5Faum1cwvNM?key=ZakVLgDvbyBcOtmV15U4Kw)

The Essentials Summary Page in nOps offers a streamlined view of your cost-saving opportunities and their realizations. Here are its key features:

- **Filters**: Customize data by date range, cloud account, and opportunity type to focus on the most relevant information.

- **Savings Summary**: Provides a quick snapshot of your potential and realized savings, including lost opportunities, estimated total savings, net savings percentage, and annualized net savings.

- **Total Savings Chart**: Visual representation of daily, weekly, or monthly potential savings categorized by EC2 Rightsizing, ASG Rightsizing, Storage, Scheduler, and Idle Resources.

- **Opportunities Summary**: Breaks down potential savings into specific categories, allowing you to identify and act on the most impactful cost-saving measures.

These features enable you to monitor, analyze, and optimize your cloud costs effectively.


## Rightsizing

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXdpuzbLf4ZD_W5J94oy20nT-dUTSLgbjZ05mMExEQ35A0ZuEih-CIJ4MsiN7FVrXbxoUozcXAditidxt4utSVTEscPjz0HCrtNvcK8VQUauliOyOccljfZ95Dld5AIUUnRH7frN7zrEY-M_cnE7pI_2k_Gu?key=ZakVLgDvbyBcOtmV15U4Kw)


### Overview of EC2 rightsizing<a id="overview-of-ec2-rightsizing"></a> ###

nOps EC2 rightsizing generates recommendations based on detailed utilization metrics collected via Datadog or the AWS CloudWatch agent. These data-backed and accurate recommendations can be verified in the nOps dashboard and applied with one click. 


### Rightsizing Recommendations<a id="rightsizing-recommendations"></a> ###

1. View the recommendations summary.

2. Select the action button to proceed with rightsizing.

3. Schedule and verify the operation.


### Datadog Agent in ASG for Enhanced Rightsizing<a id="datadog-agent-in-asg-for-enhanced-rightsizing"></a> ###

Few tools offer ASG rightsizing recommendations, despite ASGs being a huge portion of your compute costs. nOps automatically analyzes every EC2 instance in your environment (including short-lived ones) and groups them into their respective ASGs using metadata. This provides a clear view into your ASG resource consumption at an aggregate level and access to cost-saving recommendations.

For each instance within an ASG, our rightsizing algorithm compares the maximum recorded usage of key metrics against the capacity of a lower instance type, multiplied by a threshold value that accounts for potential future usage spikes. nOps takes into account the aggregate performance and utilization metrics of all instances within an ASG to make informed recommendations.

If all of the instances can be rightsized, the ASG can be rightsized. If there are several instance types, they can be analyzed and rightsized separately. 

These rightsizing recommendations are then pushed to nOps microservices, which are responsible for showing recommendations from within the nOps platform.


### Installing the Datadog Agent<a id="installing-the-datadog-agent"></a> ###

1. Create or update an EC2 Launch Template for the Datadog agent.

2. Add the user data script for installing the agent.

3. Apply the configuration and restart the Datadog agent service.


### Datadog Agent Configuration on Your Linux Based ASG ###

[Datadog Agent Configuration on Your Linux Based ASG](https://help.nops.io/Datadog-agent-configuration-on-linux-based-asg.html#installing-the-datadog-agent-for-enhanced-rightsizing-recommendation-on-the-nops-platform)<a id="datadog-agent-configuration-on-your-linux-based-asg"></a>

#### Steps to Configure Datadog Agent<a id="steps-to-configure-datadog-agent"></a>

3. Update the EC2 launch template with Datadog agent installation script.

4. Configure Datadog to prefer IMDSv2.

5. Restart the Datadog agent service to apply changes.


### CloudWatch Agent Configuration on Your Linux Based EC2 ###

[CloudWatch Agent Configuration on Your Linux Based EC2](https://help.nops.io/cloudwatch-agent-configuration-on-linux-based-ec2.html#how-to-install-amazon-cloudwatch-agent-on-an-ec2-instance-for-enhanced-rightsizing-recommendations)<a id="cloudwatch-agent-configuration-on-your-linux-based-ec2"></a>

#### How to Install Amazon CloudWatch Agent<a id="how-to-install-amazon-cloudwatch-agent"></a> ####

1. Attach the necessary IAM role.

2. Install and configure the CloudWatch agent on the EC2 instance.

3. Enable the desired metrics and confirm the configuration.



## Scheduler ##

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXesCHvasbjy6x93IumB6wqWAOyPWrUlXhgvwJ0WtlPgOZgRE6xo9gkk0725reAksNwo283bz9KEBqsuY_RfspKE5FZV3kIn4khxpicNJr_XXWdg5vDxg9oOSWHy4aCi3LXGDbJlz4loCzGz5XTngL2m3yqY?key=ZakVLgDvbyBcOtmV15U4Kw)


### What is nOps Scheduler?<a id="what-is-nops-scheduler"></a> ###

Chances are, your resources don’t need to be running 24/7. nOps Scheduler leverages EventBridge to automatically pause unused cloud resources, such as EC2 instances, and integrates with Terraform to create, manage, and monitor schedules and resources. 

Based on historical utilization data, we can identify the hours when an instance is not needed. nOps Scheduler continually provides a list of recommended resources to put on a schedule. Apply with one-click to schedule and save. 


### Summary of Steps ###

1. Identify AWS idle resources using nOps. ([Link](https://help.nops.io/scheduler-terraform.html#identify-resources-for-scheduling))

2. Create a new schedule on the nOps Dashboard.

3. Add tags to your Terraform code. ([Schedule a resource using Terraform](https://help.nops.io/scheduler-terraform.html#schedule-a-resource-using-terraform)**)**


### Creating a Schedule ###

[Creating a Schedule](https://help.nops.io/scheduler-terraform.html#create-an-nswitch)<a id="creating-a-schedule"></a>

1. Log in to your nOps account and navigate to Essentials > Scheduler > Create New Schedule.

2. Select the time zone and time to schedule the resource.

3. Apply the Terraform code to implement the changes.



## Idle Resources ##

[Idle Resources](https://www.nops.io/blog/stop-idle-ec2-instances-with-one-click/)

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXdgUCNj6Bx3Cx-SZ_vF0KJqrXc1_J1I7cupFV5m_rQBm9p7JDWWNBsKm7L54_N8T_ZZScj_nCDwWxa2YotS2-4kvoff4zkbYOxn5mVmmpq_G2oN9aVXjj2rMoMGSrMP3nkkvBbw7KRNzdNfBi5prT4kUuBO?key=ZakVLgDvbyBcOtmV15U4Kw)


{% include note.html content="Stopping idle instances is one of the most effective cloud optimization strategies. For every dollar saved on an instance, two more dollars are saved on accompanying storage, network and database charges."%}

With nOps Essentials, it’s easy to eliminate in bulk unnecessary and wasteful hidden costs like EC2 instances and EBS volumes. 


### What is an Idle EBS Volume? ###

[What is an Idle EBS Volume?](https://help.nops.io/ebs-idle-cleanup.html#what-is-an-idle-ebs-volume)<a id="what-is-an-idle-ebs-volume"></a>

An idle EBS volume is defined by nOps as a volume that has been detached from any EC2 instance for over 30 days. These volumes consume storage resources and incur unnecessary costs.


### How idle EBS volume works ###

1. Identify idle volumes using nOps.

2. Choose to either create a snapshot before deletion or delete immediately.

3. Automate the cleanup process using nOps Essentials.


### Step-by-Step Guide to Stop Idle Instances ###

[Step-by-Step Guide to Stop Idle Instances](https://help.nops.io/ebs-idle-cleanup.html#step-by-step-guide-on-how-to-stop-idle-instances-with-nops-essentials)<a id="step-by-step-guide-to-stop-idle-instances"></a>

1. Sign in to the nOps platform.

2. Access the Idle EBS Volumes Recommendations.

3. Review and confirm the cleanup process.

4. Initiate the stopping process and verify automation status.


## Storage ##

![](https://lh7-us.googleusercontent.com/docsz/AD_4nXcfNVJTdesbVqBSrE14xvrhu5WPeZA8StPyqJ3VeRST9BBjJgTiDWySM9bXyDr3ySkXfP9499iN2QMta7U9cd0XiTq8oQH8HYlHQ2sQbNhEyWBPPx2Rn098CapCjANyHPR0YD82MUwIQMlJVi45--vWtqFS?key=ZakVLgDvbyBcOtmV15U4Kw)


### Introduction ###

The Essentials Storage feature is optimize cost and performance with features like one-click EBS volume migration to GP2 to GP3 and automated snapshot cleanup.


### Why migrate your volumes with Essentials? ###
[Why migrate your volumes with Essentials?](https://help.nops.io/essentials-storage-configuration.html#why-migrate-your-volumes-with-essentials)

- Cost Efficiency: Save on storage costs by migrating to more cost-effective volumes.

- Automation: Simplify the migration process through automation.

- Performance: Improve performance with newer storage types.


### How volume migration works ###

1. Identify idle volumes for migration.

2. Use the Essentials Storage feature to automate the migration process.


### Automated EBS Snapshot Cleanup ###

[Automated EBS Snapshot Cleanup](https://www.nops.io/blog/new-automated-ebs-snapshots-cleanup/)

Automated cleanup of orphaned and unused EBS snapshots to reduce storage costs. The system identifies outdated snapshots based on criteria like age, frequency of use, and unreferenced data, and moves them to cheaper storage tiers, ensuring data access is retained while significantly lowering costs.


### How EBS snapshot cleanup works ###

{% include note.html content="nOps uses AWS APIs to pull detailed information about each snapshot’s data blocks and compares them to determine uniqueness and relevance." %}

1. Snapshots are evaluated based on age, usage frequency, and unreferenced data. Snapshots meeting the criteria for cleanup are identified and recommended for archiving.

2. Users can review and approve cleanup recommendations in bulk, archiving outdated snapshots to cost-effective storage tiers with a single click.


### Steps to Configure Your nSwitch to Optimize Costs ###

[Steps to Configure Your nSwitch to Optimize Costs](https://help.nops.io/essentials-storage-configuration.html#steps-to-configure-your-nswitch-to-optimize-costs)

1. Log in to your nOps account.

2. Navigate to the Essentials Storage section and select the volumes to migrate.

3. Follow the on-screen instructions to complete the migration.
