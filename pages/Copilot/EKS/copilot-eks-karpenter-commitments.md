---
title: Compute Copilot for EKS - Karpenter with Commitments
keywords: savings, recommendations, karpenter, compute copilot, commitments
tags: [copilot, eks]
sidebar: mydoc_sidebar
permalink: copilot-eks-karpenter-commitments.html
folder: Copilot
series: [Copilot]
weight: 3.0
---

## Existing Commitments Management ##

Using too much Spot can lead to underutilization of your purchase commitments like Savings Plans and Reserved Instances. 

To optimize utilization under a Compute Savings Plan, use the `Use As Base` feature. Simply mark a NodePool as a template in the NodePool Details by checking “Use As Base.” This signals the platform to prioritize On-Demand capacity, allowing nOps to automatically generate a new NodePool with identical specifications and integrate it seamlessly into your cluster. This feature ensures you  avoid underutilization of your commitment.

## What if you don’t want to mark a specific NodePool as a Base template? ##

In this case, we will automatically select one NodePool as a reference and **create a fully On-Demand NodePool to shift Spot to On-Demand**, causing your Commitment Plan to avoid underutilization.

This will constantly be monitored and adjusted in terms of resources to also avoid Overutilization.
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeJZPJKVkHj4ol60KRtzwk1EhkQMFpMe3gcEnVuD1mZhGyXizA88fhRxQc4DDMl2lq2jlTMPUr3-TJ7lVFC09RUhTdHsLe2DvFLFUjxfH4PNF2r3kghYzqHfZ1as4Lb9WrOeQ4m6uBGWu-PtlTsacBdjxEL?key=RYEeNWmiHRSBCVEVTH-zpA)

After nOps detects underutilization, the created NodePool will be displayed in the dashboard.

The name will consist of an UUID prefixed with \`nops-\`. 
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf-S9A-xdKWsEDiFkmDHXIGXTYPvfI0uV9VicYIU4rg2cAb8Jy5F3fewcdIEladxtZA3SWzQxU3BUdeu6ZfC3WyBCFSAA_RNzA4RobmCSrJtEJEiWxeNCFgtGfGL0CWDDloe9K4XJk7Sdy87mny0-0d0Tk?key=RYEeNWmiHRSBCVEVTH-zpA)


## What is the configuration for the Auto-Created NodePool? ##

The Auto-Created NodePool will contain all the configuration specifications from the **Base** one with some small changes which are detailed below (weight, instance-families and VCPUs limit).

This will include the metadata but without the Kube generated fields, like resourceVersion or generation.


### Weight ###

The weight of the NodePool will be set to the maximum weight of the existing NodePools + 5. This means that if the maximum current value is 90, the new weight will be 95. If the maximum value is 98 for example, the value will be capped at 100.


### Instance-Families ###

The list of instance families will be contained from the Base NodePool filtered by specified limits.


### VCPUs Limit ###

The VCPUs limit will be based on the calculated underutilization value and will be constantly adjusted to match the Commitment Plan.


### Visibility Dashboard Utilization ###

Here’s an example of how you can monitor your instances via the nOps Visibility Dashboard.

In the below picture you can find one On-Demand instance launched by the Auto-Created NodePool.
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXerFmXjikFrcVgCxQHlyFzI1EeTWwwXSzcf6Khyk2RkeyCNWx3_8xaV-0WpiezbJRnAd3748o6jMi73_FkAyldry-LVLTJbfPB5jlq64GotMZBroGxnpjyOMJbOZvJJ9GhBpZ71pq76xdgrEsTwdVxekr_W?key=RYEeNWmiHRSBCVEVTH-zpA)