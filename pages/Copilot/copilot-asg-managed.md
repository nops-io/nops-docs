---
title: nOps Copilot Managed ASG
keywords: savings, recommendations, sharesave, asg, copilot, autoscaling
tags: [copilot, asg]
sidebar: mydoc_sidebar
permalink: copilot-managed-asg.html
folder: Copilot
series: [Copilot, Onboarding]
weight: 1.0
---


# Introduction to Copilot Managed Solution for Autoscaling Groups (ASG) #

## About Copilot Managed Solution for ASG and how it works ##

- ASG configured with a `Fully Managed ASG` approach is converted to a Mixed Instances Policy (MIP) ASG, allowing nOps Compute Copilot to define Spot Instance Types this ASG can launch.
- Compute Copilot Lambda keeps Managed ASG MIP in sync with the latest Spot Market recommendations by nOps, taking into account your Reserved Instances data. As a result, during scale-out events, Managed ASG launches a Spot instance that is cheaper than the OnDemand this ASG was launching before configuring it to Compute Copilot.
- In case there are OnDemands available for Spot migration running, or there are Spot instances that are at risk of interruption, Compute Copilot Lambda initiates an Instance Refresh to bring the ASG to the state approved by nOps Spot Market recommendations.


## Why Managed ASG approach ##

In addition to [the benefits of the original](https://help.nops.io/copilot-asg-onboarding.html?#why-compute-copilot) Compute Copilot ASG approach, that replaces OnDemand instances to Spot after a launch, the benefits of this Managed ASG approach are: 

- By utilizing ASG Mixed Instance Policy, Managed ASG approach typically does not replace instances after they are launched, reducing the amount of unnecessary noise your Application experiences.
- Since Compute Copilot with Managed ASG integrates directly to AWS Instance Refresh and MIP, it now supports complicated use cases like **AWS CodeDeploy** and any other **Life Cycle Hook** specific cases.
- By setting a strict MaxSpotPrice setting within ASG MIP, it makes sure the price of Managed Spot is always cheaper than the OnDemand was initially defined, even when Spot Market Prices are changing frequently.


## Configuring your ASG to Compute Copilot as a nOps Managed ASG ##

The process of configuring an ASG to Compute Copilot as a Managed ASG is similar to the [process used in the original Compute Copilot ASG approach](https://help.nops.io/copilot-asg-onboarding.html?#steps-to-configure-your-asg-cluster), you just need to select a **Fully Managed ASG** on the ASG Configuration Screen.
