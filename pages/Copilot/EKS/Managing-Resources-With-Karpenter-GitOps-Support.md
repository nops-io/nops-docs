---
title: Managing Resources With Karpenter GitOps Support
keywords: GitOps,savings, recommendations, sharesave, nks, karpenter, compute copilot
tags: [copilot, eks]
sidebar: mydoc_sidebar
permalink: Managing-Resources-With-Karpenter-GitOps-Support.html
folder: Copilot
series: [Copilot, Onboarding]
weight: 2.0
Category: [Copilot]
---

## Managing Resources With Karpenter GitOps Support

With GitOps support, you can now manage Karpenter configurations either through the nOps dashboard, the CLI or IaC tools (such as Terraform and CloudFormation). nOps automatically detects and syncs any changes to onboarded NodePools and NodeClasses, allowing you to automatically maintain all your configurations seamlessly through your chosen tools.

## Pre-requisites

1. You must have [onboarded](https://help.nops.io/copilot-eks-onboarding.html) at least one Kubernetes cluster to nOps Copilot for EKS.

2. The nOps Karpenter agent must be on version 0.4.1 or later. 

   a. For a prefilled command, please visit the cluster details page: [Configure nOps Karpenter Agent on EKS](https://help.nops.io/Configure-nOps-Kubernetes-Agent-on-EKS.html)

# Overview of GitOps Support

 1. nOps automatically scans and displays all of your NodeClass and NodePool objects in the dashboard. 
 
     a. NodeClasses and NodePools created by nOps are marked with an icon to differentiate them from NodeClasses and NodePools that were created outside of nOps.These detected NodeClasses and NodePool objects are not automatically managed by nOps, but their configuration can be viewed in the application.

![](https://lh7-us.googleusercontent.com/mEtxOo2h5BH1DWmXJHGDG4eadLDhXV9Ar4QbmnDwdz7On3UwcABRz27GxY5SeVI5ED6-hqwfB-ny-HU0YTXHtmoQNQaR7qmBq19YWsq7RwS5-ILPenxj_UFzjhYL1bSgFiaJRn7hll_QxwLTFETB2Ig)

_nOps dashboard showing two NodeClasses created and managed by nOps, a NodeClass created outside of nOps managed by nOps, and a NodeClass created outside of nOps and not managed by nOps_

   2. A new attribute `imported_by: nops` defines whether nOps will manage 
      and update your configurations for a NodeClass or NodePool for Spot 
      optimization. If you want nOps to manage a NodeClass or NodePool that 
      was created outside of nOps, add the key:value `imported_by: nops` 
      under \`metadata.annotations\` in your source code.

   3. Once the attribute is added, the object will be automatically detected 
      in approximately one minute. 

   4. Once detection has taken place, nOps will begin to manage the 
      configuration for your NodePool and automatically optimize workloads 
      being provisioned by it. You are now able to edit the NodeClass from 
      either the nOps UI or your source code, and changes will be 
      automatically synced.   

         a.The nOps agent continually monitors the Karpenter resources 
           annotated with `imported_by: nops` and sends any changes via API 
           to our application. If you edit a NodeClass or a NodePool using 
           the nOps dashboard, the changes will likewise be propagated 
           automatically to your cluster by the nOps agent and API. 

{% include custom/series_related.html %}