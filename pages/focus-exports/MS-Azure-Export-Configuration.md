---
title: How to Set Up Microsoft Azure Focus Export with NOps Platform
keywords: focus, exports, CUR, Microsoft, MS, Azure, Configuration
tags: [focus, exports, CUR, Microsoft, MS, Azure, Configuration]
sidebar: mydoc_sidebar
permalink: ms-azure-export-configuration.html
folder: focus-exports
series: [focus]
weight: 1.0
---


# How to Set Up Microsoft Azure Focus Export with nOps Platform

This guide will walk you through the process of setting up a Microsoft Azure Focus Export and configuring it within the nOps platform. By following these steps, you will be able to generate comprehensive billing data exports from Azure and integrate them with NOps for enhanced cost management and analysis.

For your convenience, we've included video tutorials of the setup process:

- **Part 1**: [Video Tutorial](https://www.loom.com/share/fbc2acbcc8b3472c96c4e9b5f2901848?sid=07c345c0-fe6c-4e73-b3a5-26f19f2d69b6)
- **Part 2**: [Video Tutorial](https://www.loom.com/share/58d9e8e6f8ea404aa1ee845fd0373b59?sid=cdfec151-6d82-4115-98c0-f6443ce1102a)
- **Part 3**: [Video Tutorial](https://www.loom.com/share/65b4788f2bfd4112a4fe22a1ccfa4b94?sid=aac6f8af-7dbc-42cd-8993-40d1ee647f62)

---


## Prerequisites

- **Access to nOps Platform**: Ensure you have the necessary permissions to access the Organization Settings within your nOps account. If not, contact your account administrator.
- **Azure Portal Access**: You need appropriate permissions within the Azure portal to create exports and access billing scopes.
- **Global Admin Access (Optional)**: If you don't see certain billing scopes or subscriptions, global admin access may be required.

---

## Step 1: Access Organization Settings in nOps Platform

1. **Log In to nOps**: Sign in to your nOps account.
2. **Navigate to Organization Settings**:
   - Click on your profile icon or organization name in the top-right corner.
   - Select **Organization Settings** from the dropdown menu.
   - *Note*: If you do not see this option, contact your account administrator to obtain the necessary (Admin) permissions.

---

## Step 2: Configure Azure Focus Export in nOps Platform

1. **Go to Focus Exports**:
   - Within the Organization Settings, click on the **Focus Exports** tab.
2. **Initiate Azure Focus Export Setup**:
   - A pop-up window titled **Azure Focus Export** will appear.
3. **Enter Configuration Details**:
   - **Configuration Name**: Enter a recognizable name for your export configuration (e.g., "My Azure Billing Export #1").
   - **Storage Account Name** and **Storage Account Key**: These will be obtained from the Azure portal in later steps.

---

## Step 3: Set Up Focus Export in Azure Portal

### a. Access Cost Management + Billing

1. **Log In to Azure Portal**: Go to [Azure Portal](https://portal.azure.com) and sign in with your credentials.
2. **Navigate to Cost Management + Billing**:
   - Search for **Cost Management + Billing** in the search bar or find it in the services list.

### b. Select the Appropriate Billing Scope

1. **Check Billing Scopes**:
   - On the Cost Management + Billing page, ensure you are viewing the correct billing scope.
   - If you have multiple scopes or do not see your subscriptions:
     - Check the box labeled **"As a Global administrator, check this box to view all Microsoft Customer Agreement (MCA) scopes"** to display all available scopes.
2. **Choose the Billing Scope with all your subscriptions you want to export the billing data for**:
   - Select the highest-level billing scope (e.g., your billing account or profile) to ensure all subscriptions are included in the export.
   - *Note*: Creating exports at this level eliminates the need to create separate exports for each subscription, resource groups, etc.

### c. Create the Focus Export

1. **Navigate to Exports**:
   - In the sidebar, under the selected billing scope, click on **Exports**.
2. **Create a New Export**:
   - Click the **+ Add** or **Create Export** button.
3. **Configure Export Settings**:
   - **Export Type**: Select **Focus** under **Cost and Usage Focus**.
   - **Export Name**: Enter a unique name for your export (e.g., "nOpsFocusExport"). Hit "Next"
   - **Storage Account**:
     - Choose **Create New Storage Account** to set up a dedicated storage account for the export.
     - **Subscription**: Select your preferred subscription.
     - **Resource Group**: Choose an existing resource group or create a new one.
     - **Storage Account Name**: Enter a valid unique name for the storage account where you want the export to be saved (e.g., "nopsstorageaccount").
   - **Container**: Enter `nops`.
   - **Directory Path**: Enter `billing`.
   - **Format**: Select **Parquet**.
   - **Compression**: Choose **Snappy**.
   - **Additional Settings**: Leave the rest of the options as default.

4. **Review and Create**:
   - Click **Next** to review your settings.
   - Ensure all details are correct:
     - Export type is set to **Focus**.
     - Storage account and container names are as intended.
   - Click **Create** to finalize the export setup.
5. **Wait for Deployment**:
   - The creation process may take 1-2 minutes.
   - Once completed, you will be redirected back to the Exports page, and your new export will be listed.

### d. Retrieve Storage Account Name and Key

1. **Access the Storage Account**:
   - From the Exports page, under your newly created export, find the **Destination** section.
   - Click on the linked storage account name to open it.
2. **Navigate to Access Keys**:
   - In the storage account sidebar, go to **Security + Networking**.
   - Click on **Access keys**.
3. **Copy Storage Account Name and Key**:
   - **Storage Account Name**: Note or copy the name displayed at the top.
   - **Key**: Under **Key1**, click **Show keys** if hidden.
   - **Copy Key**: Copy the key under **Key1**.
   - *Important*: Ensure you copy the **Key** and not the **Connection String**.
   - *Security Note*: These keys can be regenerated at any time, and doing so will invalidate the old keys. The keys provide access only to this storage account.

---

## Step 4: Complete the Configuration in nOps Platform

1. **Return to nOps Platform**:
   - Go back to the Azure Focus Export configuration pop-up where you left off.
2. **Enter Storage Account Details**:
   - **Storage Account Name**: Paste the name you copied from Azure.
   - **Storage Account Key**: Paste the key you copied.
3. **Finalize Setup**:
   - Click on **Set Up Account** or **Save**.
   - A success message should appear, and your export configuration will now be listed in nOps.

---

## Step 5: Wait for Data Availability

- **Initial Data Generation**:
  - It may take **24 to 48 hours** for Microsoft Azure to start generating the Focus Export data.
- **Data Retrieval by nOps**:
  - nOps will check the storage account every 24 hours.
  - Once the data is available, nOps will automatically ingest it for processing and analysis.

---

## Conclusion

You have successfully set up a Microsoft Azure Focus Export and configured it within the nOps platform. This integration enables comprehensive cost management by providing detailed billing data across all your Azure subscriptions.

---

## Additional Resources from Microsoft to go in detail on FOCUS Exports and Storage Account Keys 

- **Microsoft's Detailed Guide on Setting Up Focus Export**:
  - [Create New Exports](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/tutorial-improved-exports#create-new-exports)
- **Retrieve Storage Account Keys and Name**:
  - [View Account Access Keys](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-keys-manage?tabs=azure-portal#view-account-access-keys)

---

## Support

If you have any questions or encounter issues during the setup process, please feel free to reach out to the nOps support team:

- **Email**: [support@nops.io](mailto:support@nops.io)
- **Website**: [help.nops.io](https://help.nops.io)

We are here to assist you with any concerns or additional guidance you may need.

---

*Thank you for choosing nOps for your cloud cost management needs!*
