---
title: nOps Copilot-Managed ASGs
keywords: savings, recommendations, sharesave, asg, copilot, autoscaling
tags: [copilot, asg]
sidebar: mydoc_sidebar
permalink: copilot-managed-asg.html
folder: Copilot
series: [Copilot, Onboarding]
weight: 1.0
---


# Introduction to Copilot-Managed Autoscaling Groups (ASG) #

## About Copilot-Managed ASG and how it works ##

Compute Copilot for ASGs are now fully managing your ASGs for additional savings and reliability. Here’s how it works:

- ASGs managed by Copilot are now converted to Mixed Instances Policy (MIP) ASGs, allowing nOps Compute Copilot to define Spot Instance Types this ASG can launch.
- Compute Copilot Lambda keeps Managed ASG MIP in sync with the latest Spot Market recommendations by nOps, taking into account your Reserved Instances data. As a result, during scale-out events, Copilot-Managed ASG launches a Spot instance that is cheaper than the On Demand instance this ASG was launching before configuring it to Compute Copilot.
- If there are OnDemand instances available for Spot migration running, or there are Spot instances that are at risk of interruption, Compute Copilot Lambda initiates an Instance Refresh to bring the ASG to the state approved by nOps Spot Market recommendations.


## Why have Copilot manage ASGs ##

In addition to [the benefits of the original](https://help.nops.io/copilot-asg-onboarding.html?#why-compute-copilot) approach, which includes the replacement of On Demand instances to Spot after a launch, having Copilot fully manage your ASGs brings the following additional benefits: 

- By utilizing an ASG Mixed Instance Policy, instances are typically not replaced after they are launched, reducing the amount of unnecessary noise your Application experiences.
- Since Compute Copilot now  integrates directly with AWS Instance Refresh and MIP, it now supports complicated use cases like **AWS CodeDeploy** and any other **Life Cycle Hook** specific cases.
- Setting a strict MaxSpotPrice setting within the ASG MIP ensures the price of Managed Spot is always cheaper than the OnDemand that was initially defined, even if Spot Market Prices are changing frequently.


## How to make the update ##

Simply follow the [process used to configure  Compute Copilot for ASG approach](https://help.nops.io/copilot-asg-onboarding.html?#steps-to-configure-your-asg-cluster).  and select **Fully Managed ASG** on the ASG Configuration Screen.
