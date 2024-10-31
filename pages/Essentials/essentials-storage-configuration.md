---
title: Essentials for Storage
keywords: savings, recommendations, sharesave, essentials
tags: [essentials]
sidebar: mydoc_sidebar
permalink: essentials-storage-configuration.html
folder: Essentials
series: [Essentials, Storage]
weight: 2.0
---

# Introduction #

## What is Essentials Storage for EBS volume migration GP2 to GP3 type? ##

The nOps Essentials module allows you to automatically migrate candidate volumes from gp2 to gp3 volumes via EventBridge or nSwitch Changeset.

## Why migrate your volumes with Essentials? ##

Managing EBS storage volumes can be a complex and costly task, especially when dealing with resources not controlled by Infrastructure as Code (IaC). Organizations often find themselves spending more than necessary on storage and struggling to enact bulk automated updates efficiently. This monopolizes hours of engineering time while leading to high operational costs.


That’s why we added a new feature to [Essentials for Storage](https://www.nops.io/essentials/) allowing you to automate the laborious task of changing candidate volumes from gp2 to gp3 volumes, freeing up your time to focus on building and innovation.

## How it works ##

If your resources are controlled by Infrastructure as Code (IaC), nSwitch Essentials integrates with Git and Terraform to streamline the process of identifying and fixing code related to storage optimization. This reduces manual work, making it fast and easy for engineers to take action on implementing cost-saving measures.


Utilizing nOps’ certified integration with Amazon EventBridge, nSwitch Essentials can also intelligently update configurations on resources that are not controlled by IaC. 

- This can result in up to 20% cost savings and enables organizations to efficiently enact bulk automated updates of storage.
- The savings depends on the size of GP2 volume. Not all volumes come with same amount of savings.
- When size is lower that 170gb it's 20%.
- When size is bigger it can be around 10%.
- When size is between 170 to 334 -- GP2 to GP3 migration is not cost-effective -- hence in nOps platform, volume of these sizes would not show up as recommendations.
- Lastly, nOps recommendations engine considers volume with a minimum 7 days of lifespan, and no attachment to eks/asg/etc. Only vanilla EC2 volumes are considered for this feature.


## Steps to Configure Your nSwitch to Optimize Costs ##



### Prerequisites ###
* Users must be logged in to their nOps account. 
* Your AWS account must be linked to your nOps account.
* By default, AWS has a soft limit of 50 TB in gp3 EBS Volumes per region per account. Exceeding that limit with a migration through EventBridge will cause the migration to fail. Make sure to increase this limit if the total size of GP2 EBS volumes to be migrated is more than 50 TB in an account.


### How to Increase EBS service quota ###

In case your post-migration gp3 EBS volume size exceeds that limit:

1. [Navigate to the EBS service](https://console.aws.amazon.com/servicequotas/home/services/ebs/quotas) in the quota console.
2. Search for gp3.
3. Select the radio button for [Storage for General Purpose SSD (gp3) volumes in TiB](https://console.aws.amazon.com/servicequotas/home/services/ebs/quotas/L-7A658B76) and click ‘**_Request quota increase’_**.

        

	![](https://lh7-us.googleusercontent.com/vre-sDvnF4L69U4KLvhOme6gli1FvE97mIAouQBwjduEPDI7jaIhDfbP8A9Il9WQr5KNCCPu-eYBR-0o6V1z-QjLDhUOaOMJLpBVCBd06SOnXWKFiuExRYa0wj5Qzzuz8zYbxshKtzmbCMh_YBuokK7cK_wcuMSIv1C_ziLwIPtAwZ7yM2sWOwKxysg87w)


4. In the Change Quota box, enter the total amount that you want the quota to be.

	![](https://lh7-us.googleusercontent.com/UhvTEwp-Vm3MBOg6mY1I5mZXOZHUy8VnrsR3OfyWVQ3iFoCIhJ5kELO_0o7RrbDrvsUj5JpDaDYC6oVnitBw7iVRpKVfa-znUxYf4iGsFBrumW7DbPjU0d819Tc81cxqLXt3Jvx_E-BM3DL9NYIl_SokgGEDZx1kzNrxcHSCN96LJWvzKHiFiAQ1VkHOIw)




5. Click ‘**Request’**.
6. Repeat this for each region and account for which you are planning on migrating gp2 to gp3.

### Essentials EventBridge for cost optimization (non IaC managed resources) ###

nOps is a certified partner event source in Amazon EventBridge. Create an EventBridge in your AWS account with just one click to deploy a small Lambda in your account via the CloudFormation template. 

nSwitch Essentials analyzes your data and presents recommendations that you can act on with confidence.  Schedule the optimization to happen now or later. nOps’ scheduling engine will securely send a signal through Amazon EventBridge to trigger the optimization.



![](https://lh7-us.googleusercontent.com/MOfjgtoW8Tnv_QDq2-0LTWudvqbK5xSZ4D439lI2zCGDPp4Od3Y65IxUB5xBkX-H1ZZKzG2sI71JP5C-ygO3Yi6dfakih2Qmq-BUUhAGUyfsajNIOkNMR70v8cVr6pZyxDJvGW-KTPvcRZTvx-YIfKL6oS7x1vMO0sxs7r48JnyG6Oqam3XKYhq7waysow)


				   Architecture Diagram

Steps : 



1. Create EventBridge for Essentials
* Navigate to the **Organization settings > Integrations > EventBridge**
* Provide EventBridge with a unique name and select an AWS account.
* Choose a ‘**Region’** and create.

![](https://lh7-us.googleusercontent.com/eWs7HOTD9zpg7EyGynYDPqhehUwKihyC0qpc0JW9JO1MZXF8wrgINcMPWwgNiGjf8C_t3-STmKq_oqjdBciIhA_H9LxT3P291zDEYDXH1ZYxbCaHF__SuaOuDxA54hm6tgtLTh2w1aGG5wm-9BsqLhGYMZIbDC0p8Qrh95KRDNxKUP98DzTVQc-mp2uW8g)


**Note**: You can select multiple AWS accounts and create the EventBridge in all selected accounts. **



2. Configure the created EventBridge for Essentials
* Navigate to the **Organization Settings** > **Integrations** > **EventBridge**
* Click the ‘Launch Stack’ button in the ‘Essentials’ row for the required account. 

![](https://lh7-us.googleusercontent.com/YAFwDim8kN8JLnCMusgISE3qOu8Lj7cxALFRYaf6HbxRfjm347d1kwZ51PNE4QP-F4hufvI64oFk3w5mFWccuXQpkacdWHcCULMXEctCJs3ZxByR4rAFO2xP90ZtTyML77igmqdGiYgQTKAJG-j2K_3CKWhEy1GBxolzCyB0nYT_fGSu_-zC16bw4LKNHA)

**Note**: If You have created an EventBridge in multiple AWS accounts, you can follow the one-click Cloud Formation method to configure EventBridges in all accounts with a single Cloud Formation run using a stackset.



3. Navigate to **nSwitch** > **nOps Essentials** module
* In ‘**Storage Recommendations’**, select any recommendation and open the detail page to configure.
* You can edit the ‘**nSwitch name’**
* Select the ‘**Schedule pattern’** now or later
* By default, all resources in the recommendation are selected. You can select/deselect as desired.
* Finally, hit ‘**Create nSwitch’** and you will be redirected to the** ‘Resource nSwitch’** tab.

![](https://lh7-us.googleusercontent.com/weVdFB3zUV0EIU0Y2lvjsrAfx57gUvgZTfmpT_7Suzb5HF5pSKWs7LMAkFpZMxpkteSqn2xO0GOEq-hkkl-RodoR0Dn1UrJnUdY_kVEyoGhlXxPIaRCnYBliluHjmXen3hqPVB45aQ2MV949TYrDfDeKYM9oeSEmZWLM4fsvNKNLhNE3rfY9a4HMMUvcgg)



### nSwitch Essentials Changeset approach to cost optimization (Used for IaC managed resources) ###

* The feature works in two stages. First, it connects to your platforms and reads your data. It then identifies repositories and detects branches and state files within these repositories. This process is quick and seamless, with the system intelligently grouping recommendations and applying changes.

![](https://lh7-us.googleusercontent.com/6bf1bSjVtp4ANybtwP-KdDL3lJOvii9rbbw6Hfta4UEWEQEtDEq856SzGrvUxTib9E-9dhDlNPJ-NCZASqYYOcD4pyk4MqeCzMqN-6cX_7Oi0AJ4tfOG5jS-4mWq-KJQZIwbGX_9ROo4XhiDBGBIcJuey_dvoYINDaSbO0i0y_WvtOxpk2NMtY5DedaO0Q)


Steps : 



1. Connect your GitLab or GitHub with nSwitch Essentials. We will then scan your chosen repositories for Terraform files and identify S3 buckets containing Terraform state files.
* Navigate to **Organization Settings** > **Integrations** > **nSwitch Changeset**
* Configure Gitlab with your nOps accounts so nOps can scan your chosen repositories for Terraform files, Identify S3 buckets containing Terraform state files.
* You will be redirected to the Gitlab page where you can see the Gitlab integration success message.
* Repositories will be shown. Choose the one with Terraform files and click **next**.


![](https://lh7-us.googleusercontent.com/IXWqHBy4ZtYByPH7uIQAuWdlRKqkmba5Xjc5xU9fxOcH0ShfoufCietuNV8_Jovv3L_UPV94G3k26m9dakk-6TPhgWsJ9dL5rRTkb3MmINFZRBLefc3a7UZR41xeXotVBOIHmAxWxqJYesQqFrydQB2vWLdiocvbubr7llReOuxaY6q96g0g9inGX7JVLg)


**Note:**


* Make sure that you are logged into your Gitlab account in the same browser before you click on ‘Integrate Gitlab’.
* After a successful Gitlab integration message, it can take around 3 to 5 minutes and a browser refresh for the changes to reflect in our platform.

2. You can see 2 sections: ‘**Repositories’** and ‘**Terraform state files’**. 
* You can choose an available branch from the drop-down, save it, and switch to Terraform state files. [In the following example we chose the master branch].
* In the Terraform state file section, you can see the AWS account. Here,  you need to perform a ‘**Launch stack**.
* You will be redirected to the AWS console for the Cloud Formation process.
* Once the CF run is completed, nOps will analyze your data and generate the recommendation shortly. 

![](https://lh7-us.googleusercontent.com/Ej5ftIbj_tDyxVduH-Xtbv9OGmayfSuQMCTEASl7yCxAG7AVZC5iBsAgyrm7obrOur7l3PqfF7Hl-jg9yn6CgdPkR6Z-xA9M4Zeh4rzZT1zzCXOdfzfrx7QhGUfXgy2yk_I4rZwO4ySHsNfL8uXrWbpatlyRw2VHcqVMJLyIncfhgSkHe9iOGqTb9tN6Ww)



3. Go to **nSwitch** > **nOps Essentials** and choose the recommendation to create changeset nSwitch.
* Click on ‘**Action’** to jump to the detail page where you can configure your nSwitch by entering a name and selecting resources. 
* Upon confirmation, click on ‘**Convert to gp3’. **You will be redirected to the nSwitch dashboard with the new nSwitch created.

![](https://lh7-us.googleusercontent.com/g3cSQuv5BILUxNo8FGYagDjboEGqHoLJUpGLe-T_cRLAbF7wWYqxM-habhIO31WXC5VUTSzdZsMyop6yH_QiKSXsi5wuvOIY-IWVgAuGd_39e8N2EzW_ehYm40JaftYkop0SffGHJHnGS1YkWC4UoQeW9Pk0xStEB809-Brt0olRPfTj4MdTGk2W4CVXsQ)

**Note**: If you don’t see nOps changeset type in the selected recommendation, then you can’t create nSwitch for the changeset. In this case,  available resources in the recommendation must be migrated using Eventbridge. 



4. By default, nSwitch status would be ‘**changeset submitted’**
5. Once nOps generates the PR in your Gitlab account, the generated PR link can be viewed from the nSwitch dashboard beside the Action column, which redirects the user to the Gitlab account’s merge requests section. In this case, the nSwitch status would be ‘**changeset created’**.
6. Users can simply review and merge the changeset into their codebase to optimize storage and savings. 


<br/><br/>

{% include custom/series_related.html %}
