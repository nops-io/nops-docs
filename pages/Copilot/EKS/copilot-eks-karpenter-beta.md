---
title: Compute Copilot for EKS -  Karpenter Beta Support
keywords: savings, recommendations, karpenter, compute copilot
tags: [copilot, eks]
sidebar: mydoc_sidebar
permalink: copilot-eks-karpenter-beta.html
folder: Copilot
series: [Copilot, Onboarding]
weight: 3.0
---

# Compute Copilot for EKS: Karpenter Beta Support #

Compute Copilot is an intelligent workload provisioner that continuously manages, scales, and optimizes your EKS compute for the best cost and stability. Copilot is built on Karpenter for its latest-gen scheduling capabilities and seamless integration with cost-effective Spot Instances. Learn more about how Copilot can put your EKS cost optimization on auto-pilot [here](https://www.nops.io/compute-copilot/).


## About Karpenter Beta Support ##


With Karpenter Beta Support, you can now use Copilot with any EKS cluster, regardless of which version of Karpenter you are using. 


## Pre-requisites ##

1. You must have [onboarded](/copilot-eks-onboarding.html) at least one Kubernetes cluster to Copilot for EKS.


## Overview of the Update ##

### Version identification ###

- The Copilot agent reads the installed version of Karpenter from charts and sends it via API to our application. Based on the installed version, the UI will ensure that the client sees and interacts with the proper resources.

### nOps agent and API update ###

- We have updated the agent installed on client clusters. This agent is responsible for sending the CRUD commands to the Kubernetes API, and choosing the proper CustomResources (NodeTemplates/NodeClasses or Provisioners/NodePools).

- We have enhanced our API to validate the CustomResources based on the version. Therefore we added a new set of validators for the NodeClasses and the NodePools.

{%include note.html content="In order to benefit from the v0.33 version or newer, as a Karpenter user, you’d have to first upgrade to v0.32 and then go on to higher versions."%}

## How nOps Supports Karpenter Beta Release Changes ##

The Karpenter .33 Beta Release brings significant changes — you can learn more about them here. This documentation will cover the most important changes and how they are handled by Copilot. 

### Migration to v1beta1 API ###

The v0.33 release transitions from the alpha API to the beta API. This advancement involves significant updates to the structure of configuration files and resources, progressing Karpenter towards a more stable and mature API while focusing on backward compatibility to minimize breaking changes.

Copilot pulls the current Karpenter version of each cluster in order to decide whether to go with the new or old resources. If you have multiple clusters with different Karpenter versions, each will be handled individually to benefit from the proper version support.

Use Case Scenarios:

1. UseCase 1: An application that is mult-client oriented, with Karpenter versions different from client to client. 

2. UseCase 2: A client with multiple clusters, and various Karpenter versions installed on each. This client has already created many custom resources and wishes to migrate only specific EKS clusters in accordance with their needs.


### Updates to Helm Chart Values ### 

The transition to v1beta1 has brought about critical deprecations and updates in Helm chart values. It's crucial to ensure that these values are updated during the migration to v1beta1.

We have updated our agent in the backend configuration, allowing it to install the right components and update all new terminology for you. It requires running this quick helm chart command: 

```bash
 helm upgrade -i karpenops oci://public.ecr.aws/nops/karpenops-helm --version 0.2.1 --namespace karpenter --set apiKey=<your nOps API Key> --set clusterId=<your nOps cluster ID> [--set datadogKey=ou812-abcd-1234-3d14-0a1b2c34567d]
```

### Replacement of Provisioners with NodePools ###

The concept of Provisioners in Karpenter has been replaced with NodePools, which necessitates updates to existing configurations. 

With the new update, we handle the automatic conversion from Provisioner to NodePool in your YAML file.

![](https://lh7-us.googleusercontent.com/cK8R42DDfP-wEvLAaQwoVU4CQQ3okwAdaVDkqRdKDjORhZDdEy_cL1lVT01fsm8KaxXK5fKeLmfOZmIrozNXL57KrccP1XQByDUFtcSmHgYZy3Y6M3rSvzok0DhkrmM6pl0sVGyB5Se6vAhI-acQxnY)

Please note the following Karpenter compatibility matrix:

- Up until **v0.31** (included) only Provisioners and NodeTemplates are allowed as Custom Resources

- **V0.32** is a version that supports both Provisioners + NodeTemplates and NodePools + NodeClasses

- From **v0.33** onward, Karpenter expects and it is working only with NodePools and NodeClasses

### Replacement of AWSNodeTemplate with EC2NodeClass ###

Similarly, AWSNodeTemplates have been replaced with EC2NodeClasses. This change requires a conversion of existing templates to the new format, which is likewise automated in the new update.

![](https://lh7-us.googleusercontent.com/HW1xAsMUdB6HG_spPXsboNhNosWRQpD4olbDtj9ZUG2aOvBfYjNNXiOPSK3Z5PfmTkiExZcsA-3RtX7Pgt3lLeg5YRlHspvWY6AYT0wY2byOvk5Cj4vnwDD-kHE2d0SxGeuu1TWIClVlajOfZxJJ1QA)

### Rolling Over Nodes ###

Karpenter .33 Beta introduces a new “disruption” field where you can specify policies for consolidation, offering more control over the lifecycle and management of nodes within a cluster. To move to the new NodePool, users now have new options like periodic rolling with expiration, forced deletion, or manual rolling.

With the nOps update, you can now use “disruption” with the following values:

**“WhenUnderutilized”/ “WhenEmpty”**: when nodes are underutilized, i.e. empty  and not containing pods
**“ConsolidateAfter”**: the amount of time to wait after discovering a consolidation decision
**‘ExpireAfter”**: the amount of time a node can live on the cluster before being removed

### Updating Workload Labels ### 

In the interest of streamlining and aligning with the latest configurations, older labels have been deprecated and need updating as part of the migration process.

{%include important.html content="labels are coming under a different structure and key and need to be set up properly. For further detail, consult the Karpenter documentation."%}

![](https://lh7-us.googleusercontent.com/KsX-gJRVTvRH9tea64hX7PfHaIlsWILbLzTpparm3fwUPleC8KnXAB6rmxqlWxBrDMz7tzFUQ-ADRTpLsDsVcBU1r3J9_q9BFvWZlJL4cei-gBn3x10v_4vQ8yM23Bh2ZXwgHzP8GzIFU3T8hRDbYQk)

**_And the new CustomResources:_**

NodeClass:
 ![](https://lh7-us.googleusercontent.com/mscIjpYFCvgDKCe3B9rdBU71QqXwf98MWo8FYMd_jeIDYUWQGNtOpIIvwYQTVO33oESev1esQiFmUA0lBZo4cnn8LVvfqG6k8pge2YA2VkcWbZhamnSpsRuyUu-F6ui-eoG0P1K_onbYfCl5c3ApqSw)


NodePool:
 ![](https://lh7-us.googleusercontent.com/fE3D2L_fkseLk2j5UUdzK7cssewIme6hOFBebKDS5xACNYkGmSQhdMGOzqxVMHvXabHB1WZOWXreOD4xKr1BnXUhCVTRhfk0irThiTJIlKFuBUPhOzGyjzVFpg6Rc7SGbMH-351rFj1FifmSKGWp5NI)


{% include custom/series_related.html %}