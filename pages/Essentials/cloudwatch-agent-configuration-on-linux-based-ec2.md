---
title: Cloudwatch Agent Configuration On your Linux based EC2
keywords: savings, recommendations, cloudwatch, essentials, rightsizing
tags: [essentials]
sidebar: mydoc_sidebar
permalink: cloudwatch-agent-configuration-on-linux-based-ec2.html
folder: Essentials
series: [Essentials, Rightsizing]
weight: 2.0
---

## How to Install Amazon CloudWatch Agent on an EC2 Instance for Enhanced Rightsizing Recommendations



This document provides step-by-step instructions for installing and configuring the CloudWatch agent on a Linux-based EC2 instance, to enable memory metrics for nOps Enhanced Rightsizing Recommendations.



**Prerequisites**

Before proceeding with installation, ensure the following prerequisites are met:



- IAM role on EC2 with assigned permission of "CloudWatchAgentServerPolicy"

- Attach IAM Role to EC2



If the IAM role’s permission is not configured, follow the below steps 



- Create an IAM Role 

- IAM > **Roles** and **Create role**

![](https://lh7-us.googleusercontent.com/a5vwpwARNkohxDXNvjP6lMaA_fBOKwZyiiCNl_pBe9e8cTpOk32eIXN1pSmHerVVngluFw0RMqxCJectWOXAVaRpWllzXQIg4SXuVvQ0vPY6wXPbkVg6rNInEtmdc_VbzB3_CcKw7HW3D_hru7T-2wM)



- Service or use case SELECT EC2 and click **Next**

- **In** **Permissions policies** select **CloudWatchAgentServerPolicy** and click **Next**

![](https://lh7-us.googleusercontent.com/-Z8BFk-ipj_X0mAplqbMtBogIzDWw66CMO4m2B0K7XTf2ZcXz-yu61v9jl9SUDNUJNAGXYaQAAuHus9dK2lL_IptiBuK1v1wRHm5--PllW3hlGMs7njFFW-jAvfwLVzABdfoWt_EMi0praYh1MeEU6s)

- Enter role **Name** and **Save** 

To Attach the IAM role to the EC2 instance, follow the following steps: 



- Select EC2 from the action menu, select **Security** and **Modify IAM Role**

![](https://lh7-us.googleusercontent.com/RqCCo58ZHgN88Rl5pLf99g50rjCAGSI6p6unth2CXZSH7seoFenZVlC_VKBBokfBR05rOZ88mTu_Ps1rUX3eRkFmMpjdRZOBIPzjqO89qSBldqc5oRHj6Y6vVlyMp1TGc2M7OBo8vp15aAnty8ZLvAk)



**Note: if you have already any IAM role attached to the EC2 instance, then you just need to add the "CloudWatchAgentServerPolicy" permission for that role.**
Also, This technical document is based on  Amazon Linux. The commands may be different for Ubuntu.



**Installation Steps**



1. **Connect to the EC2 Instance**: Log in to the EC2 instance using SSH or AWS CLI.



2. Ensure that AWS CLI is installed using syntax:

    **aws --version**



This command checks the version of the AWS CLI installed on the instance.



3. To Install CloudWatch Agent:



- Run following the command in the shell 
<b>
    sudo yum install amazon-cloudwatch-agent
</b>

<br>
- Confirm the installation when prompted:

* Is this ok \[y/n]: y



![](https://lh7-us.googleusercontent.com/D4QzWSTJt2nQuBoVUMiLYBWPOTBu8NYyn5omgqyQGRey__ydFkNXBE-su0uqG1clL7KGJrWsfWzXOp_afYbluZOomnPlx33A9gREkpyWH9uI984aNd1OsFmJrJGTrpub498E5PZiAjs7mnWM1dzHASQ)



4\. Execute the configuration wizard using the following command:

<b>

    sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard
</b>
Once the wizard starts, follow the configuration steps provided below.



**Configuration Steps**

- **OS Selection**: Choose "Linux" as the operating system.

- **Environment Type**: Select "EC2" for the hosting environment.

- **User Setup**: Choose a user with the necessary access to run the agent.

- **StatsD Daemon**: Enable StatsD daemon with default configurations.

- **Metrics Collection**:

  - **CollectD**: Choose "No" if not using CollectD.

  - **Host Metrics**: Yes

  - **CPU Metrics**: Enable CPU metrics per core.

  - **Memory Metrics**: Enable monitoring of memory metrics.

  - **EC2 Dimensions**: Add EC2 dimensions to all metrics if available.

  - **Resolution**: Collect metrics at high resolution (sub-minute resolution).

  - **Default Metrics Configuration**: Choose "Basic" for detailed metric configuration.

* Confirm the configuration settings and proceed with the wizard.



![](https://lh7-us.googleusercontent.com/EwYlvdA7fBWsB-YezvsX_uuYecfSMwpOjWuR_jHP5NuYWKimm_XpgWj3OwEUGc5W3Vv1yauHDyeimdfc86MPivZ9dqKLa4JecUoX2fQBMRo9GEqIbgP8ddNDd1VqYkqZuXjbjGCwKN3Kn_ZnWazJFFQ)



5\. Ensure the status is active.

<b>

    sudo amazon-cloudwatch-agent-ctl -a status
</b>

![](https://lh7-us.googleusercontent.com/0na5RRc3cfAT9epXg-oLaR3lbjaGoIPyvEjEerR1UtGRz8wEYf3Khjf6gZePZS9kM7eOVn7RFdvr60A-45Yjy8XYH5It7kg9SzLkN8pARA6c4qlPm2HAR4F7-5F0arYqB-GbQoqMfTczkXa4m_H28k4)



6. To start CloudWatch Agent



    **sudo amazon-cloudwatch-agent-ctl -a start**

![](https://lh7-us.googleusercontent.com/cnfp7BbGz-uAMPWOK4XQrTVeOpaCc63Qf_67XFRbgBpoobcT0FpeLBN4LozyQYeuw7BeclENQwNOiWVhOiB_XUEKXRl6sm9_edOfy96ML_tc7jx2ejvFc4VqAoy6jzPCC6g4bx59LzpJfqDEx9gWdCY)



**Note:** Following successful configuration, it is important to note that there may be a delay of a few hours before the matrix reflects accurately on CloudWatch. This delay is attributed to various background processes and data synchronization procedures.



7\. Viewing memory metrics in the CloudWatch Console:

- Go to the **AWS Management Console**

- Navigate to **CloudWatch** > **Metrics** in the left sidebar.

- Under **Browse**, select **CWAgent** or **EC2** depending on your setup.

- Choose the specific instance for which you want to view memory metrics.



![](https://lh7-us.googleusercontent.com/xnIQABgZVM3BgI7lclvmgGn50SQnxMcuv15OTjiPWnjUP5PUHMhHFVS8DdI5ZAqNQWda785SYOhPQx9qUFIjMjrEXqn0Cg_xHKlPVn4rRf9RnGqwnadIBsz5zS2Ddo8qV1Y_dt2A-oV45ZaKJxXbrgs)



**Note**: Upon the display of the memory matrix on AWS, the enhanced recommendation functionality will undergo automatic synchronization with nOps. This synchronization process will be triggered once a minimum dataset of 360 hours becomes available within a 30-day timeframe, sufficient for rightsizing analysis.



**Troubleshooting:** 



**I have already installed the CloudWatch agent on my EC2 instance, but I don't see memory metrics available. What could be the issue?**

If you have already installed the CloudWatch Agent but are not seeing memory metrics, there are several possible causes. Below are steps to troubleshoot and resolve the problem:

1. Check IAM Role Permissions:

   1. Ensure that the IAM role assigned to the EC2 instance has the necessary permissions.

   2. Verify that the IAM role includes the "CloudWatchAgentServerPolicy" permission to allow the CloudWatch Agent to collect and publish metrics to CloudWatch.

2. Verify CloudWatch Configuration:

   1. Review the CloudWatch Agent configuration file (/opt/aws/amazon-cloudwatch-agent/bin/config.json) to confirm that metrics such as memory are enabled for collection. Use the following command: 

       **nano /opt/aws/amazon-cloudwatch-agent/bin/config.json**



- And verify the below code should be in the config file:

        "mem": {
                "measurement": 
                [             
                 "mem_used_percent"
                ],
                 "metrics_collection_interval": 60
                },

![](https://lh7-us.googleusercontent.com/X8_i8XcJs8caRA-fw8BoOD4DtMqUyUhuJA2YCBZdJn4T7h1iq0HkjPwOikIGAqWCZNqtpr1QxrVxtr_FYo2f6kdud_-QLFHdM9G9KQ0SahB0WzF7IIKAGmuLH6CufzD05d3wFfddJbTkBxgh4nLvWGE)


<br/><br/>

{% include custom/series_related.html %}