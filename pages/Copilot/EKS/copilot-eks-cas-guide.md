---
title: Onboarding your EKS clusters to Copilot for EKS Cluster Autoscaler
keywords: savings, recommendations, sharesave, nks, karpenter, compute copilot
tags: [copilot, eks]
sidebar: mydoc_sidebar
permalink: copilot-eks-cas-guide.html
folder: Copilot
series: [Copilot, Onboarding]
weight: 1.0
---


# Enabling Compute Copilot for EKS Clusters with Cluster Autoscaler: Onboarding Guide

# **About Compute Copilot for Cluster Autoscaler and how it works**

Compute Copilot is a powerful service designed to automatically optimize compute-based workloads, reducing costs on AWS EKS by intelligently migrating EC2 instances to Spot instances using machine learning algorithms.

*Cluster Autoscaler* is a key feature that dynamically adjusts the number of nodes in a node group within an EKS cluster to accommodate changes in resource requirements for pods. When a scaling decision is made, Cluster Autoscaler communicates with the associated Auto Scaling Group (ASG) to adjust the **`DesiredCapacity`**, prompting the ASG to automatically scale the cluster nodes.

Compute Copilot for Cluster Autoscaler fully manages the ASGs that belong to your cluster for additional savings and reliability. Here’s how it works:

![](/tmpimg/managed_asg_diagram.png)

- ASGs managed by Copilot are now converted to Mixed Instances Policy (MIP) ASGs, allowing nOps Compute Copilot to define Spot Instance Types this ASG can launch.
- Compute Copilot Lambda keeps Managed ASG MIP in sync with the latest Spot Market recommendations by nOps, taking into account your Reserved Instances data. As a result, during scale-out events, Copilot-Managed ASG launches a Spot instance that is cheaper than the On Demand instance this ASG was launching before configuring it to Compute Copilot.
- If there are OnDemand instances available for Spot migration running, or there are Spot instances that are at risk of interruption, Compute Copilot Lambda initiates an Instance Refresh to bring the ASG to the state approved by nOps Spot Market recommendations.

By combining the automation capabilities of Cluster Autoscaler with the intelligent instance management of Compute Copilot, this solution offers a seamless and cost-effective approach to optimizing AWS EKS workloads and reduce costs.

# Why Use Copilot to Manage EKS ASGs

- Compute Copilot uses AI-driven decision making to provision and run auto scaling group instances at the cheapest price in real time, without manual effort.
- Compute Copilot allows you to benefit from Spot savings with the same reliability as on-demand. By analyzing historical data and Spot Termination events, it ensures your critical workloads remain safe from interruption. 
- Compute Copilot does not require your workload to be transferred to a proprietary system, but works directly with AWS ASG.
- By utilizing an ASG Mixed Instance Policy, instances are typically not replaced after they are launched, reducing the amount of unnecessary noise your Application experiences.
- Since Compute Copilot for Cluster Autoscaler integrates directly with AWS Instance Refresh and MIP, it supports complicated use cases like **AWS CodeDeploy** and any other **Life Cycle Hook** specific cases.
- Setting a strict MaxSpotPrice setting within the ASG MIP ensures the price of Managed Spot is always cheaper than the OnDemand that was initially defined, even if Spot Market Prices are changing frequently.

# Prerequisites

1. You must be logged in to your [nOps account](https://app.nops.io/accounts/signin/).
2. Your AWS account must be configured to your nOps account.
3. You must have an EKS Cluster with Cluster Autoscaler installed.

# ****Steps to Configure Your EKS Cluster for Cost Optimization and Savings****

{%include important.html content="
The EKS Dashboard distinguishes clusters utilizing Cluster Autoscaler by representing them in orange, while clusters employing Karpenter are represented in blue. For learning how to configure clusters that use Karpenter, please, refer to this [this documentation](https://help.nops.io/copilot-eks-onboarding.html)."%}


## Install Compute Copilot Lambda

For instrunctions on how to install Compute Copilot Lambda, please, refer to [the Compute Copilot for Auto Scaling Groups (ASG) documentation](https://help.nops.io/copilot-asg-onboarding.html?#prerequisites).

## Configure ASGs associated with the Cluster

1. Navigate to Compute Copilot → EKS from the nOps dashboard.
2. Choose the EKS cluster you want to cost-optimize.
3. Navigate to the Cluster Configuration TAB.
5. Select the ASG you want to configure:
    - Open Configure Modal. The details section of Auto Scaling Group will be prefilled. 
    - The AWS Lambda configuration section will show the version detail and current status of the Lambda Stack to confirm it is properly configured on the AWS account.
    - You can create or choose an existing ASG template in the Spot detail section.


{%include note.html content="Unlike Compute Copilot for Karpenter, Compute Copilot for Cluster Autoscaler does not require an agent. The Helm command for installing an agent, which you can find in the configuration tab for clusters using the Cluster Autoscaler, is for installing the BC+ agent required for container utilization metrics and rightsizing."%}

6. Create an ASG Template:
    - Give the ASG template a unique name.
    - Select the CPU architecture based on the AMIs of the ASG you are going to attach the template to.
    - By default Dynamic min VCpuCount & MemoryMiB is checked, setting the minimum vCPU and RAM requirements based on the size of the On-Demand EC2 instance being replaced.You can disable this option and set the CPU or Memory suitable for your workload from the Instance Requirements list.
    - You can also directly choose instance families. Compute Copilot will select the most optimal choice for price and stability out of the provided options.
    - Now click **Create**
7. (Optional) Set the **Minimum Number of On-demand Instances**
**Minimum Number of On-Demand Instances** defines the number of On-Demand instances that should be left in the ASG and not replaced with Spot. ASG Lambda won’t do any On-Demand to Spot replacement if it has fewerf On-Demands instances than specified in this config setting.
8. (Optional) Set the **Spot percentage** and **Max Spot Instances** using the draggable bars.
    - **Spot percentage** defines the desired percentage of spot instances.
    - **Max Spot Instances** defines the maximum number of Spot instances to be created by Compute Copilot.
9. Select **Fully Mananged ASG**.
    - Compute Copilot for Cluster Autoscaler only works when **Fully Mananged ASG** is selected.
10. Click **Configure** and repeat these steps for all ASGs in the cluster.

![onboarding-ca.gif](/tmpimg/onboarding-ca.gif)

# What to Expect After Configuring your Cluster

Compute Copilot will start running your workloads on Spot Instances. You can navigate to the cluster's dashboard to view the instances being launched by Compute Copilot.

![dashboard-ca.gif](/tmpimg/dashboard-ca.gif)

{% include custom/series_related.html %}