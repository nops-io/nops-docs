---
title: Compute Copilot For ASG Support Matrix
keywords: savings, recommendations, sharesave, copilot, asg
tags: [savings, recommendations, sharesave, copilot]
sidebar: mydoc_sidebar
permalink: copilot-asg-configurebytag.html
folder: ShareSave
series: [ShareSave, Copilot, Onboarding]
weight: 2.0
---



# Configuring ASGs by Tag to use the Compute Co-Pilot ASG
 <a id="configuring-asgs-by-tag-to-use-the-compute-co-pilot-asg"></a>
=====================================================================

### The new nOps feature Configure By Tag offers increased ease-of-use to users of Compute Copilot. Onboard your ASG to Copilot with Configure By Tag for the following benefits:<a id="the-new-nops-feature-configure-by-tag-offers-increased-ease-of-use-to-users-of-compute-copilot-onboard-your-asg-to-copilot-with-configure-by-tag-for-the-following-benefits"></a>

- **Integration with any IaC tool**, such as Terraform.

- **Multi-configuration capabilities**: Configure multiple ASGs at the same time with just a couple of clicks, for quick and easy onboarding.

- **Effortless Configuration**: Configure by Tag feature helps clients who frequently modify Auto Scaling Groups (ASGs) save significant time. It eliminates the need to manually go to the nOps dashboard and reconfigure ASGs each time they create, destroy, or recreate ASGs. Tags are automatically managed, for a seamless and time-efficient process.


### Integrate nOps Compute Copilot with your ASGs by adding the following Tags:<a id="integrate-nops-compute-copilot-with-your-asgs-by-adding-the-following-tags"></a>

|                                       |                                                                              |              |
| :-----------------------------------: | :--------------------------------------------------------------------------: | :----------: |
|              **Tag Key**              |                                 **Tag Value**                                | **Required** |
|         nops\_copilot\_enabled        |                                     true                                     |       ✅      |
| nops\_copilot\_launch\_template\_name | Name of the Launch Template you created in the Compute Copilot ASG Dashboard |       ✅      |
|  nops\_copilot\_max\_spot\_instances  |                                 Integer (0-N)                                |       ❌      |
|  nops\_copilot\_max\_spot\_percentage |                                Integer (0-100)                               |       ❌      |

nops\_copilot\_max\_spot\_instances - Maximum number of Spot instances that Compute Copilot will place as part of your Auto Scaling Group

nops\_copilot\_max\_spot\_percentage - Maximum percentage of Spot instances that Compute Copilot will place as part of your Auto Scaling Group.

_Note: if you delete the Compute Copilot Launch Template used in one or more ASGs, these ASGs will no longer be processed by nOps Compute Copilot._


### Step-by-Step guide to Configure by Tag:<a id="step-by-step-guide-to-configure-by-tag"></a>

1. Navigate to **Compute Copilot** -> **Auto Scaling Groups** -> **Manage ASG Templates**

2. Create ASG Template

![](https://lh7-us.googleusercontent.com/UouzjCSllCtBUg3s7lukfPzEK7iMk_TX-vpnRrJEeK0TeiIwzrECGTydSsnEA9pk1DLZW7W2ek0v0hlpVjzyAZrr87Pa02SazL7NZTemNn8xZ4_dBhHKjJOyd36pOtBxKFPCjCztXAvgy5fT9l0PWO0M1lx1obmwGe4mKefIzYT9R-82XdrUEfKY2iK5yA)

3. Give the ASG template a unique name

4. Select the CPU architecture based on the AMIs of the ASG you are going to attach this template with

5. (Optional) Exclude unsupported instances for ENI Trunking 

6. Choose the instance families, vCPU, and Memory suitable for your workload. From this pool, Compute Copilot will select the most optimal choice for price and stability.

7. Create and Copy the name of the template


### ![](https://lh7-us.googleusercontent.com/o7TFEg-g9LX6FRMiNALyQudCaET-RRf5YfSfUIvrGxNuGvdU0QBa_5s_MQPUwaPP8jCUJwKFSl6EfbDSZRdx8rFPTi-1ieERzpW0zQlVS-cHYle8-XaGtl_0rOXI8rO5gTo5EBbiNqfBNo091vmEUSu18P8oweRDW38RT_-jIJtcdgmN-OFChrGDq8mjDw)

8. Attach “_nops\_copilot\_enabled = true”_ and “_nops\_copilot\_launch\_template\_name =_ _\*name you have copied from above\*”_ tags to your ASG 

9. Optional tags are _“nops\_copilot\_max\_spot\_instances”_ and _“nops\_copilot\_max\_spot\_percentage”_

![](https://lh7-us.googleusercontent.com/IFrv5Wl_1dC4f0uWTjTeGslLpFrDEvKX5kmS6m2Zoh-jhNuxzgUd5xQVK_SEBNe8ncaZ-7wMrQmj6LT5h13mZVQy4H7PgLqEPkBDz12GWKNVR0-YgaVsUEkVJswIRVc0wV8UKhmxzBkA131HKgRGLeDbxpWlDvainf1aFoERWPnBBQmimpbdyKvgm7J6-Q)

10. After creating the ASG using IaC with the tags specified, within a few minutes the ASG will reflect a “connected” status with the all the appropriate information (template name, Spot percentage / number of Spot instances) 

![](https://lh7-us.googleusercontent.com/GBg8CBWhCFtkBbWWJ-QaGMp_rj8VU7J87IgTLrofKH6jtdXs1Svf3H82wSUvxu57PzpTCwxaNW-alfyaQL9AZwSwAqqhYUAF-oDZfSSb8tsQZz6OpGbEBnxxvkO8NeNpWvOHwNIX82znGiKTGpm75nKXq1SSfxNzXerIL7_eHSndUDtkl-Pxj9UckYQ9ug)

## FAQ

1\) What happens to my ASGs if I delete the Launch Template in nOps Compute Copilot?

I_f you delete the Compute Copilot Launch Template used in one or more ASGs, these ASGs will no longer be processed by nOps Compute Copilot._\
2\) Why don’t I see Spot instances as soon as I create the ASG using IAC while leveraging Configure by Tag ?

If a reliable and cheaper Spot is available in the Spot market and in the same AZ, it will take up to 30 minutes to see a Spot instance as part of your ASG.\
3\) Can I use both the tags “nops\_copilot\_max\_spot\_percentage” and “nops\_copilot\_max\_spot\_instances” on the same ASG?

Yes, you can use both tags. 

4\) What happens if I don't use either of the tags “nops\_copilot\_max\_spot\_percentage” and “nops\_copilot\_max\_spot\_instances” on my ASG?

If you don’t use either tag, by default, Compute Copilot will try to replace all your On-Demand instances as part of ASG to the Spot instances


{% include custom/series_related.html %}
