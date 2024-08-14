---
title: Configuring ASGs by Tag
keywords: savings, recommendations, sharesave, copilot, asg
tags: [copilot, asg]
sidebar: mydoc_sidebar
permalink: copilot-asg-by-tag.html
folder: Copilot
series: [Copilot, Onboarding]
weight: 2.0
Category: [Copilot]
---



# Configuring ASGs by Tag to use the Compute Co-Pilot ASG #
 <a id="configuring-asgs-by-tag-to-use-the-compute-co-pilot-asg"></a>

## Introduction ## 

The new nOps feature Configure By Tag offers increased ease-of-use to users of Compute Copilot. Onboard your ASG to Copilot with Configure By Tag for the following benefits:<a id="the-new-nops-feature-configure-by-tag-offers-increased-ease-of-use-to-users-of-compute-copilot-onboard-your-asg-to-copilot-with-configure-by-tag-for-the-following-benefits"></a>

- **Integration with any IaC tool**, such as Terraform.

- **Multi-configuration capabilities**: Configure multiple ASGs at the same time with just a couple of clicks, for quick and easy onboarding.

- **Effortless Configuration**: Configure by Tag feature helps clients who frequently modify Auto Scaling Groups (ASGs) save significant time. It eliminates the need to manually go to the nOps dashboard and reconfigure ASGs each time they create, destroy, or recreate ASGs. Tags are automatically managed, for a seamless and time-efficient process.

## Prerequisites ##

1. You must have already configured the appropriate Lambda roles for Copilot as per [these instructions](/copilot-asg-stackset.html)

## Integrate ASGs by tag ##

Integrate nOps Compute Copilot with your ASGs by adding the following Tags:<a id="integrate-nops-compute-copilot-with-your-asgs-by-adding-the-following-tags"></a>

| **Tag Key**                                  | **Tag Value**                                                                                                                   | **Required** |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------|
| nops\_copilot\_enabled                       | true                                                                                                                            | ✅            |
| nops\_copilot\_launch\_template\_name        | Name of the Launch Template you created in the Compute Copilot ASG Dashboard                                                    | ✅            |
| nops\_copilot\_max\_spot\_instances          | Integer (0-N)                                                                                                                   | ❌            |
| nops\_copilot\_max\_spot\_percentage         | Integer (0-100)                                                                                                                 | ❌            |
| nops\_copilot\_replace\_protected\_instances | true                                                                                                                            | ❌            |
| nops\_copilot\_min\_ondemand\_number         | Integer (0-N)                                                                                                                   | ❌            |
| nops\_copilot\_fully\_managed\_asg           | true                                                                                                                            | ❌            |
| nops\_copilot\_spot\_strategy                | One of [`LowestPrice`, `CapacityOptimized`, `CapacityOptimizedPrioritized`, `PriceCapacityOptimized`]. Default is `LowestPrice` | ❌            |

**nops\_copilot\_max\_spot\_instances:** Maximum number of Spot instances that Compute Copilot will place as part of your Auto Scaling Group

**nops\_copilot\_max\_spot\_percentage:** Maximum percentage of Spot instances that Compute Copilot will place as part of your Auto Scaling Group.

**nops_copilot_replace_protected_instances:** This tag allows ASG Lambda to replace the instances that have Scale-in protection or its instances are protected from Scale-in. 

**nops_copilot_min_ondemand_number:** This tag sets the minimum number of On-Demand instances that should be left in the ASG. ASG Lambda won’t do any replacements (On-Demand to Spot) if it has less number of On-Demand instances than specified in the Tag value.

**nops_copilot_fully_managed_asg:** This tag makes nOps Compute Copilot [manage your ASG](https://help.nops.io/copilot-managed-asg.html).

**nops_copilot_spot_strategy:** Defines the Spot Allocation Strategy Copilot Lambda is going to use when it launches a Spot instance.

**Note:** if you delete the Compute Copilot Launch Template used in one or more ASGs, these ASGs will no longer be processed by nOps Compute Copilot.


## Configure by Tag:<a id="step-by-step-guide-to-configure-by-tag"></a>

1. Navigate to **Compute Copilot** -> **Auto Scaling Groups** -> **Manage ASG Templates**

2. Create ASG Template

![](https://lh7-us.googleusercontent.com/UouzjCSllCtBUg3s7lukfPzEK7iMk_TX-vpnRrJEeK0TeiIwzrECGTydSsnEA9pk1DLZW7W2ek0v0hlpVjzyAZrr87Pa02SazL7NZTemNn8xZ4_dBhHKjJOyd36pOtBxKFPCjCztXAvgy5fT9l0PWO0M1lx1obmwGe4mKefIzYT9R-82XdrUEfKY2iK5yA)

1. Give the ASG template a unique name

2. Select the CPU architecture based on the AMIs of the ASG you are going to attach the template to

3. By default “Dynamic min VCpuCount & MemoryMiB” is checked, setting minimum vCPU and RAM requirements based on the size of the On-Demand EC2 instance being replaced. You can disable this option and set CPU or Memory suitable for your workload from the Instance Requirements list.

4. (Optional) When selecting instance requirements, eligible instance families will be highlighted based on your criteria. To simplify the selection process, you can choose all highlighted options by clicking on the "Select All Eligible Instance Families" link.

5. You can also directly choose the instance families. Compute Copilot will select the most optimal choice for price and stability out of the provided options.

6. Create and Copy the name of the template
    ![](https://lh7-us.googleusercontent.com/CugpeTPCXgu_JhjIOZpTfvmhf3S4apoKadmbRb06m6qC-G8uC18ldhdOTe7XRuipBl8fDAREeJJCznfrzs1kQ0M1H5ZAmJvw75GH1RmWKweo2_cj0i5cPezKLSx3VoYrJ8nkZXCTWAHEI3uz3t8ExIc)

7. Attach “_nops\_copilot\_enabled = true”_ and “_nops\_copilot\_launch\_template\_name =_ _\*name you have copied from above\*”_ tags to your ASG 

8. Optional tags are _“nops\_copilot\_max\_spot\_instances”_ and _“nops\_copilot\_max\_spot\_percentage”_

    ![](https://lh7-us.googleusercontent.com/IFrv5Wl_1dC4f0uWTjTeGslLpFrDEvKX5kmS6m2Zoh-jhNuxzgUd5xQVK_SEBNe8ncaZ-7wMrQmj6LT5h13mZVQy4H7PgLqEPkBDz12GWKNVR0-YgaVsUEkVJswIRVc0wV8UKhmxzBkA131HKgRGLeDbxpWlDvainf1aFoERWPnBBQmimpbdyKvgm7J6-Q)

9. After creating the ASG using IaC with the tags specified, within a few minutes the ASG will reflect a “connected” status with the all the appropriate information (template name, Spot percentage / number of Spot instances) 

    ![](https://lh7-us.googleusercontent.com/m3VEDEamTGKq7UGUUM1-GfBAsTCjkPAwqx_JNeMpMP9JqciG285R0WkY8W4oqW3P3diXvyO04owaoUwHcI6YI_uQ7LZAOXSF0lJ2v5qsZvko8zX21iI0Oz4lzvnZr3XeHtRZmRk5sjnVBsjKFHoWk84)

## FAQ

### 1. What happens to my ASGs if I delete the Launch Template in nOps Compute Copilot?
If you delete the Compute Copilot Launch Template used in one or more ASGs, these ASGs will no longer be processed by nOps Compute Copilot.

### 2. Why don’t I see Spot instances as soon as I create the ASG using IAC while leveraging Configure by Tag?
If a reliable and cheaper Spot is available in the Spot market and in the same AZ, it will take up to 30 minutes to see a Spot instance as part of your ASG.

### 3. Can I use both “nops\_copilot\_max\_spot\_percentage” and “nops\_copilot\_max\_spot\_instances” tags on the same ASG?
Yes, you can use both tags. 

### 4. What happens if I don't use either of the tags “nops\_copilot\_max\_spot\_percentage” and “nops\_copilot\_max\_spot\_instances” on my ASG?
If you don’t use either tag, by default, Compute Copilot will try to replace all your On-Demand instances as part of ASG to the Spot instances

### 5. What happens if I don’t use the tag “nops_copilot_replace_protected_instances” on my ASG protected with Scale-In protection?
If you don’t use the tag, by default it is set to FALSE, so Compute Copilot will not replace protected instances due to Scale-In protection.


{% include custom/series_related.html %}
