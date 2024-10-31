---
title: Onboarding your EKS clusters to Compute Copilot for EKS Karpenter
keywords: savings, recommendations, sharesave, nks, karpenter, compute copilot
tags: [copilot, eks]
sidebar: mydoc_sidebar
permalink: copilot-eks-onboarding.html
folder: Copilot
series: [Copilot, Onboarding]
weight: 1.0
---

## Onboarding your EKS clusters to Compute Copilot for EKS ##

### Why use Compute Copilot EKS? ###

Learn more about how Compute Copilot for EKS can help you to put your EKS cost optimization on auto-pilot [here](https://www.nops.io/compute-copilot/).

### Pre-requisites: ###

1. You must be logged in to your [nOps account](https://app.nops.io/accounts/signin/). 
2. Your AWS account must be configured to your nOps account.
3. You must have Kubernetes cluster with Karpenter installed.

### About Compute Copilot for EKS: ###

Karpenter offers significant advancements in intelligent scaling, cluster awareness, and customization. However, it is often tuned as a one-and-done — whereas your ecosystem and market pricing are constantly changing. 

That’s where Compute Copilot for EKS comes in, constantly tuning your configuration for the best price and availability on autopilot. It allows engineering teams to take advantage of Karpenter’s more effective and granular scaling functionalities, for a fraction of the effort. 

Compute Copilot for EKS ensures you are scheduled on the most cost-optimized and stable option at every moment — automatically updating your provisioning on the fly if not for 50+% EKS savings.

### Why Compute Copilot for EKS over legacy Cluster Autoscaler: ###

Optimizing your AWS costs is a key challenge today. Spot can be cheaper, but terminations pose the threat of outages to your critical workloads. RIs and Savings Plans require risky, long-term commitment and can lead to paying for capacity you don’t need.

Compute Copilot for EKS continuously rightsizes, reconsiders, and re-evaluates your workload placement to available RIs, SPs, or Spot to maximize your savings on autopilot with complete reliability. It is aware of your entire dynamic AWS ecosystem and market pricing in real time. 

Compute Copilot for EKS even covers your commitments with a 100% money-back guarantee. If your usage changes, you change instance regions, or even leave the cloud, we buy back those commitments from you. You get all the cost savings of commitments, with all the flexibility of on-demand. In contrast, Cluster Autoscaler requires much more of your time and labor, lacks Karpenter’s advanced autoscaling automation, and makes you choose between cost savings and flexibility.

## Steps to Configure Your EKS Cluster for Cost Optimization and Savings ##

### Install nOps Agent: ###

1. Navigate to Compute Copilot → EKS from the nOps dashboard.
2. Choose the EKS cluster you want to cost-optimize.
3. Open detail view by clicking on configure button or click on cluster name and use cluster configuration section.
4. Generate a new API key for nOps Agent. 
5. Copy the custom command and run it in your command line. 
6. Test Connectivity of nOps Agent in the Compute Copilot for EKS Dashboard.

    ![](https://nops-help-site-assets.s3.amazonaws.com/EKS/Agent+Installation+new+.gif)

### Create EC2NodeClass/AWSNodeTemplate:

EC2NodeClass/AWSNodeTemplate can be created in two ways-

i. Auto Configuration

   - For the selected EKS cluster, Select create a EC2NodeClass/AWSNodeTemplate

   - Assign a unique name to it.

   - Choose the AMI Family from the dropdown menu.

 ![](https://nops-help-site-assets.s3.amazonaws.com/EKS/Auto+Node+class+Part1+.gif)

   - Add Subnet IDs manually or with Search by tags. 
   - Add Security Group IDs manually or with Search by tags 
   - For EC2NodeClass, you have to select the IAM Role to be used. In case you're not seeing the desired role in the list, you can manually insert your role name.
   For AWSNodeTemplate, you have the option to select the InstanceProfile you want or let Karpenter handle the defaulting


![](https://nops-help-site-assets.s3.amazonaws.com/EKS/Auto+Node+class+Part2.gif)

  - Configure Metadata Options with user data to give commands after node starts [optional].
  - Create Device Mapping by providing necessary details [optional].
  - Select Create Automatically.

    ![](https://nops-help-site-assets.s3.amazonaws.com/EKS/Auto+Node+class+Part3.gif)

{%include note.html content="you can create multiple node templates"%}


ii. Manual Configuration

   - For the selected EKS cluster, select create a EC2NodeClass/AWSNodeTemplate.

   - Insert the YAML code and validate it. Make sure you specify an unique name under metadata.name property for your resource
   - Now select Create manually.

![](https://nops-help-site-assets.s3.amazonaws.com/EKS/Manual++Node+class+.gif)

{%include note.html content="In the NodeClasses list you can find resources created without nOps if there are any. These are going to be shown without the nOps icons and will be available only in Yaml format in View Mode. When creating NodePools, you can select any NodeClass as reference, either created via nOps or directly created in the cluster by you or your team."%}



### Create NodePool/Provisioner: ###

i. Auto Configuration

   -  For the selected EKS cluster, select create NodePool/Provisioner.

   -  Check Use As Template (Optional, also a cluster may have at most 1 template)

   -  Assign a unique NodePool/Provisioner name.

   -  Select the created EC2NodeClass/AWSNodeTemplate to pull configuration from.

   -  Select Availability Zones.

   -  Select Capacity Type — Spot, On Demand or both \[recommended to select both]
   
   ![](https://nops-help-site-assets.s3.amazonaws.com/EKS/Auto+Node+Pool+Part+1.gif)

   - You have the option to select Max Limit of vCPUs & Memory. Using 0 in both fields will indicate Karpenter to apply no limits when allocating resources.

   - Create Taints and Labels if required \[for specific provisioner service].

   - For instance selection section, you have the option to filter the types by Architecture, Generation, Accelerator details, Networking, Storage and others. After the filtering is done, you can click Select All Eligible Instance Families for autoselection.

   - Set the weight (optional)

   - Now select Create Automatically.

   ![](https://nops-help-site-assets.s3.amazonaws.com/EKS/Auto+Node+Pool+Part+2.gif)

{%include note.html content="You can create multiple Provisioners, but each Provisioner will have only 1 Node Template" %}

ii. Manual Configuration

   -  For the selected EKS cluster, select create NodePool/Provisioner.

   -  Check Use As Template (Optional, also a cluster may have at most 1 template)

   -  Insert the YAML code and validate it.
      Make sure you specify an unique name under metadata.name property for your resource and also add the correct reference to the desired NodeClass under spec.template.spec.nodeClassRef.

   -  Now select Create manually.

![](https://nops-help-site-assets.s3.amazonaws.com/EKS/Manual+Node+Pool+.gif)

### Use As Base ###
By checking “Use As Base,” this signals the platform to prioritize On-Demand capacity and prevents underutilization of discount commitments.  You can read more about that [functionality here](/copilot-eks-karpenter-commitments.html).

### Test Connectivity ###
Once the Provisioner is created, the user can again Test Connectivity to confirm that the EKS cluster is configured correctly.

![](https://nops-help-site-assets.s3.amazonaws.com/EKS/test+connectivity+new.gif)



As soon as cluster status displays **CC Configured**, Compute Copilot for EKS will start its magic to generate savings on the connected EKS cluster. 

## FAQ ##

1. Is Karpenter mandatory to install EKS Compute Copilot?
* Yes, currently EKS Compute Copilot works only if the clusters are being managed by Karpenter. We do have Cluster Autoscaler in our roadmap but it is not available as of now.
2. Do EKS Compute Copilot Karpenter provisioners take precedence over my own Karpenter provisioners?
* Yes. Once onboarded to Compute Copilot, the provisioners already existing in the cluster are not going to be used anymore. But you don’t necessarily need to delete them. It’s just that they will not be used while Compute Copilot provisioners are there.
    **Note**: It may not always be true depending on the existing provisioners configuration.
3. Can we import the YAML for existing node templates and provisioners to configure Compute Copilot EKS provisioners?
* Yes. That is absolutely possible and is shown in the above document within the “Manual Config” steps. By clicking on Manual Config, you can just copy-paste your existing YAML templates to configure nOps Karpenter provisioner
4. Does nOps provide DevOps support to customers migrating from cluster autoscaler to Karpenter?
* nOps has experienced engineers who provide free of cost support to all clients to help them with Karpenter migration. You can get as much on-call support as you need to review your Karpenter settings. We also conduct monthly “Karpenter Office Hours” with our existing customer audience to build a community where we can share our experience, customers can ask questions in an open forum, see hands-on to some basic Karpenter implementation practices and much more. However, nOps is not a Service company and therefore, we do not take responsibility for your migration project or provide dedicated DevOps resources or do managed services. Our support is limited to only providing guidance and reviews.
    **Note**: If you want a recording of the most recent “Karpenter Office Hours” then please drop us an email on ‘support@nops.io’.
5. Does nOps Compute Copilot for EKS support configuration of Multiple Provisioners while setting up nOps Karpenter provisioner?
* Yes, absolutely. Setting up multiple provisioners is a very common thing that customers ask for and we have made it very user friendly to configure that. You get two options to do it, configure in the UI using Auto Config or configure via existing YAML template import using Manual Config method.
6. I have multiple applications running on the clusters which have different instance requirements, can Compute Copilot support different instance types in such cases?
* Yes, Compute Copilot does allow you to pick & choose only those instance types that you’d want nOps Karpenter provisioner to provision.
7. Is it possible to set minimum threshold values for metrics like CPU and Memory while configuring my Karpenter provisioner in Compute Copilot?
* Yes, we allow users to set the minimum CPU and Memory metrics
8. Can I put my Stateful workloads on EKS Compute Copilot?
* EKS Compute Copilot does not come with any limitation on Stateful workloads. However, we do not recommend putting Stateful Workloads on Spot Instances if they are running mission critical operations.
9. How can I identify resources managed by Compute Copilot EKS?
* Compute Copilot EKS adds a common tag to all resources it manages. This is accomplished by automatically adding a tag in the `spec.tags` section of AWSNodeTemplates. The value of the tag is `nops:nks:enabled=true` and it will appear on all EC2 managed by Compute Copilot EKS.

{% include custom/series_related.html %}

