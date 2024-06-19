---
title: EventBridge Integration
keywords: reporting, integrations
tags: [agents_integrations]
sidebar: mydoc_sidebar
permalink: eventbridge-integration.html
folder: Integrations_and_Agents
series: [agents_integrations]
weight: 3.0
---

EventBridge Integration makes it easier for nOps to automate workflows in the client’s environment.

1. TOC
{:toc}

# Integrating with EventBridge

Amazon EventBridge is a serverless event bus that makes it easier to build event-driven applications at scale using events generated from your applications, integrated Software-as-a-Service (SaaS) applications, and AWS services.

With EventBridge integration, nOps will be able to:

* Automate events based on nOps rules.
    
* Trigger automation to reduce the size of underutilized EC2 instances.
    
* Purchase or exchange RI automatically risk-free, if RI utilization is not optimized. To see the nOps list of risk-free commitments, go to **ShareSave Dashboard > List of Opportunities > List of Risk-Free Commitments**.
    
* Turn on and off the EC2 and RDS instances in a group with the Resource Scheduler.
    
* Trigger messages in multiple services other than the event messages.
    

To add nOps as an EventBridge partner, go to **AWS > Amazon EventBridge >** **Partner event sources**, and select **nOps** as one of the options to listen. Click on **Setup** and configure the options.

Once the EventBridge integration is set up, you can create event sources directly from the nOps application and deploy them to any account and region that you’ve connected to nOps.

To integrate your AWS EventBridge with nOps, log in to nOps and click **User Avatar > Organization Settings > Integrations > EventBridge**. In the _Event Bridge_ tab click **\+ Create EventBridge**.

On the **Create New Event Bridge** page:

1.  Create a name for this EventBridge.
    
2.  Select the AWS account you want to deploy the EventBridge into. In the AWS accounts list, you will only see the accounts that you onboard into nOps.
    
3.  Select the region you want to deploy the EventBridge into.
    
4.  Click **Create**.
    

When you click **Create**, nOps will deploy the EventBridge into your selected AWS account and region by creating an event bus in the selected AWS account.

To see the event bus that nOps just created, go to **AWS > Amazon EventBridge > Event buses > Custom event bus**.

The next step is to add an EventBridge target into Webhooks.

# Creating EventBridge for Multiple AWS Accounts #

nOps supports the creation of EventBridge for multiple AWS accounts simultaneously. In the "Create New Event Bridge" window, select multiple accounts from the dropdown to configure EventBridges across several AWS accounts at once.

![](https://lh7-us.googleusercontent.com/NgqptOG06zJcGOOp3dcVQ-jmEzFWCTkUmUdYgHv1kTEtJkalZieemnXdo_2oBerihVFNLUe_SNosAxFE8fNkP2G0btUV0kc-_U2BZW56OZqcFFRaPYTsIgvVtcPLDiiuESdQY3sHI3v77T0qdIgTWbU)

# Adding EventBridge Target into nOps Webhooks # 

nOps has a Webhook for almost every cost optimization rule in the nOps environment.

When you create a Webhook, anytime an associated event is fired a message will be sent to either the endpoint you define or the EventBridge that you create.

To add an EventBridge target into nOps Webhooks, go to **User Avatar > Organization Settings > Integrations > Outgoing Webhooks**. In the _Outgoing Webhooks_ tab, click **\+ Create New Webhook**

In the **Create New Webhook** page:

1.  Create a **name** for the Webhook.
    
2.  Select an **Event**. Notice that the Event option has two fields, in the first field you need to select the event category, and in the second you need to select the actual event.
    
3.  Select an **Endpoint**, it can be either a simple Endpoint (Target URL) or an **EventBridge**. In the scope of this documentation, we are only going to select the EventBridge option. Selecting this option will send the Webhook to your EventBridge.
    
4.  Select an EventBridge from the list. The list consists of all EventBridges that you create in **nOps > Organization Settings > Integrations > EventBridge** tab.
    
5.  Click **Save**.

When you create an EventBridge in nOps, nOps will automatically create an event bus in your selected AWS account. To see the event bus that nOps automatically created, go to **AWS > Amazon EventBridge > Event buses > Custom event bus**.
