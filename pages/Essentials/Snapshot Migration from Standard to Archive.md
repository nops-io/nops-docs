---
title: Snapshot Migration from Standard to Archive in nOps Essentials
keywords: savings, recommendations, sharesave, essentials
tags: [essentials]
sidebar: mydoc_sidebar
permalink: Snapshot-Migration-from-Standard-to-Archive.html
folder: Essentials
series: [Essentials, Storage]
weight: 2.0
---

## Guide to Using Snapshot Optimization in nOps Essentials

The Snapshot optimization feature in nOps Essentials allows for the efficient migration of your EBS snapshots from the standard tier to the archive tier, helping you optimize storage costs. 


### Understanding EBS Snapshots Archive

The AWS EBS Snapshots Archive is designed for the low-cost, long-term storage of snapshots that are seldom accessed and don't require quick retrieval.

- **Default Storage**: Initially, any new snapshot is stored in the Amazon EBS Snapshots Standard tier. These snapshots are incremental, meaning only the changes since the last snapshot are saved, keeping storage efficient and cost-effective.

- **Archiving Process**: When you choose to archive a snapshot, it transforms from an incremental to a full snapshot and moves from the standard tier to the archive tier. A full snapshot includes all the blocks written to the volume at the time the snapshot was taken.

- **Accessing Archived Snapshots**: If you need to access an archived snapshot, you can easily restore it to the standard tier, where it becomes available for use like any other snapshot in your account.

- **Cost Efficiency**: The archive tier can reduce your snapshot storage costs by up to 75% for snapshots stored for 90 days or more, provided they are rarely accessed.

**Information:**

Before using the Snapshot Archive feature, ensure the following:

- **KMS Configuration**: If you are trying to archive an encrypted snapshot, ensure that AWS Key Management Service (KMS) is properly configured. Without KMS configuration, the snapshot will not be archived. [Click Here](https://help.nops.io/essentials-stack.html#run-additional-stack-for-kms-keys-access)

- **Snapshot and AMI Associations**: Detach a snapshot associated with an Amazon Machine Image (AMI) before scheduling the archive. The archive process will not be initiated for snapshots associated with AMIs.

**Follow these step-by-step instructions to use this feature.**

 Step 1: Accessing the Snapshots Archive Feature Navigate to Storage

- Click on the **Essentials** tab from the Main Menu.

- Select **Storage** from the dropdown menu.

- In the **Storage Optimization** section, click on the **Snapshot Recommendations** tab to view recommendations

![](https://nops-help-site-assets.s3.amazonaws.com/snapshots/1.png)

 Step 2:  **Reviewing Snapshot Recommendations**

**1. The summary section displays:** 

- **Total Annualized Savings** which are estimated annual savings if you migrate all recommended snapshots.

- **Total Monthly Savings:** The estimated monthly savings if you migrate all recommended snapshots.

- **Total Recommendations:** The total number of recommended snapshot migrations 

- **Total Volumes**: The total number of EBS volumes associated with the snapshots.

**2. Recommendations Table:**

 The table below the summary lists specific snapshots for migration:

-  **Account**: The associated AWS account.

-  **Volume ID**: The unique identifier of the EBS volume.

-  **Resources:** The number of snapshot resources to Archive.

-  **Resource Type**: The type of resource (e.g., Snapshots).

-  **Region**: The AWS region where the snapshot is stored.

-  **Annualized Savings**: Estimated annual savings.

-  **Monthly Savings**: Estimated monthly savings.

-  **Description:** Action description (e.g., "Update from standard to archive tier").

-  **Action:** Button to initiate the migration process.

 Step 3: **Initiating Snapshot Archive**

1\. **Select a Snapshot for Migration**:

Click the Action button (**arrow icon**) on the snapshot recommendation you want to migrate.

2\. **Review Expected Savings and Settings**:

   You will be redirected to a configuration page.

**Review the details:**

- **AWS Account**: The account associated with the snapshot.

- **Target**: The target storage type for migration.

- **Schedule**: Choose to schedule the migration immediately or later.

- **Summary**: Displays estimated savings and volume details.

- **List of Resources**: Lists the snapshots selected for migration with details such as snapshot ID, size, and age.

![](https://nops-help-site-assets.s3.amazonaws.com/snapshots/2.png)

3\. **Confirm Migration:**

  Verify the details and click Archive Snapshots to start the migration.

![](https://nops-help-site-assets.s3.amazonaws.com/snapshots/3.png)

 Step 4: **Monitoring and Managing Migration**

1\. Migration Status:\
The Storage Optimization Summary tab will show the status of scheduled migrations.   After initiating migration, you can monitor the progress in the Storage Optimization 

section.

        2. **Scheduled Migrations:**

If you selected "Now" as the schedule option, the migration will start on trigger after 10 minutes.

You have the option to cancel the schedule before it triggers by clicking the Remove button in the Action column.

![](https://nops-help-site-assets.s3.amazonaws.com/snapshots/4.png)

   3. **Savings Realization:**

     After successful migration, savings will be reflected with the next data ingestion which 

might take up to 48 hours on the Essentials Summary Realizations tab.

  

 **Important Notes**

  **KMS Configuration:** Ensure that KMS is properly configured if you are archiving encrypted 

snapshots. Without KMS, the snapshots will not be archived.

**Detach Snapshots from AMI**: If a snapshot is associated with an Amazon Machine Image (AMI), make sure to detach it before scheduling the archive. The archive process will not start if the snapshot is still attached to an AMI.

**Conclusion**

Using the Snapshot Tiering feature in nOps Essentials helps you manage and optimize your storage costs effectively. By regularly reviewing and acting on snapshot recommendations, you can maintain a cost-efficient AWS environment. The automation capabilities further enhance efficiency by reducing the need for manual intervention.

For further assistance, refer to the nOps Essentials documentation or contact nOps support.
