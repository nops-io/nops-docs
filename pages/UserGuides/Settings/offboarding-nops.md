---
title: Offboarding from the nOps platform
keywords: setup, offboarding, tutorials, walkthrough, cancel
tags: [offboarding]
sidebar: mydoc_sidebar
permalink: offboarding-nops.html
folder: GettingStarted
series: [Onboarding]
weight: 9
---

## Offboarding from the free nOps platform ##

When customers onboard to nOps, a Cloudformation stack is deployed within the management account that:
1. Creates an S3 bucket
1. Creates a Cost and Usage Report (CUR)
1. Creates an IAM role with attached policies

After that process takes place, a Cloudformation Stackset is deployed.  This is done to create an IAM role with attached policies to gather resource information needed to power our optimization recommendations.

{% include note.html content="If you onboarded before July 2024, it's likely you created the stackset manually. "%}

## Remove the AWS assets created - before July 2024 ##
To roll-back the resources created by nOps, perform the following actions from the management account:
1. Delete the Cloudformation Stacks from the Stackset deployed during onboarding
1. Delete the Cloudformation Stackset for linked account configuration
1. Delete the Cloudformation Stack for the payer account configuration

## Remove the AWS assets created - after July 2024 ##
To roll-back the resources created by nOps, perform the following actions from the management account:
1. Delete the Cloudformation Stack for linked account configuration
1. Delete the Cloudformation Stack for the payer account configuration

Deleting the Stack(sets) will remove the resources created by nOps, severing our access to your infrastructure.

## Unsubscribe from Marketplace ##

Deleting the resources from your AWS account won't change the status of your Marketplace Subscription.
    
You will need to go to your AWS console in order to unsubscribe from nOps [https://console.aws.amazon.com/marketplace/home](https://console.aws.amazon.com/marketplace/home)


## How nOps purges data ##

The data purge policy describes how we purge data after you delete an account and how we remove data.

### Deleting an account from nOps ###


1. A client admin will be able to delete AWS accounts from the nOps Organization Settings page.
    
1. When the admin deletes an account within nOps, the account will be scheduled for deletion within 30 days.
    

![](/tmpimg/account-delete.png)

### When the account deletion task is executed ###

When account deletion occurs within the platform, the following takes place:

1. All data fetched from AWS API calls is deleted
    
1. All cost data ingested to nOps is deleted
    
1. User login information (email address and password) is retained
    

After a Marketplace subscription is unsubscribed, regardless of account deletion within the platform, a tenant purge request is triggered after 30 days.

{% include note.html content="You may also request a data purge via support ticket to expedite the process. "%}



### When tenant deletion queue task is executed ###
When the tenant deletion task is executed, it:

* Deletes all data fetched from AWS API calls.
    
* Deletes all cost data ingested to nOps.
    
* Deletes all other information associated with the tenant, such as login information (e-mail addresses, user names)
    

For more information on our Privacy Policy please visit here: [nOps Privacy Policy](https://nops.io/privacy-policy/)

If you'd like our help offboarding, please create a support ticket.

<br/><br/>
