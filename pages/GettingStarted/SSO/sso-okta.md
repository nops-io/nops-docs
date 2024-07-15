---
title: Okta SSO Integration
keywords: sso, setup, onboarding
tags: [onboarding, sso]
sidebar: mydoc_sidebar
permalink: sso-okta.html
folder: GettingStarted
series: [Onboarding, SSO]
weight: 4.0
---


# How to use Okta SSO for nOps #


To enable **single sign on** for Okta users, you must configure an Application Integration in Okta and enter information into nOps

Information on creating an Application integration for Okta is also available through [Okta online help.](https://help.okta.com/en/prod/Content/Topics/Apps/Apps_App_Integration_Wizard_SAML.htm)

After set up is complete, first-time users may need to confirm login permissions using the confirmation email.

You will need to copy and paste information from nOps into Okta and then from Okta to nOps. For ease of use, we recommend that you open 2 browser windows and log into both Okta and nOps as an Admin user.

The process contains the following steps:


## Step 1: Information you need from nOps ##


Before you can set up nOps as an App in Okta you will need the following information from nOps.

1.  Log into nOps as an Administrator user.
    
2.  From nOps, open **Organization Settings** and navigate to the **SSO** integration page.
    
3.  Enable the **SSO** setting by using the toggle.
    
4.  Select **Okta** from the dropdown list.
    
5.  Copy URL information from the following fields in nOps, to add to OKTA in the next step.
    
    * **Assertion Consumer Service**
        
    * **Entity ID**
        
    
    ![](https://lh7-us.googleusercontent.com/wYGHhwhEpswiiBGO2bFp_vM1x_caJdr4PF87Uh1sW7HpC8RtScXiX1hAYroc8KGhsUrvW3GJX19o3d_unuoyf3ojZWoxdrbtFLl1oHv2wQEhUaFtcSsXpXmQK3rAF50hB3uArFKMr8lbr2_GCotbB7g)
    

## Step 2: Setup Okta SAML 2.0 Application ##


1.  Log into to Okta as an Admin user and navigate to **Applications** page
    
2.  Click **Add Application**
    
    * For platform select **Web**.
        
    * For sign on method, select **SAML 2.0**
        
    
    Instructions for creating SAML app integrations are also available on the [Okta website](https://help.okta.com/en/prod/Content/Topics/Apps/Apps_App_Integration_Wizard_SAML.htm).
    
3.  In **General Settings** enter an **App name** and click **Next**.  
    App name suggestion: **nOps.**
    
4.  In the SAML settings page, enter information from nOps copied by you in Step 1:
    
    * In the Okta **Single Sign On URL** field enter **AssertionConsumerService** URL from nOps.  
        Select the: **Use this for recipient URL and Destination URL** checkbox.
        
    * In the Okta **Audience URI (SP Entity ID)** field, enter **EntityId** information from nOps.
        
    
    ![](/tmpimg/saml-settings.png)
    
5.  Add the following **Attributes** and **Group Attribute** from the **Attribute Statements (Optional)** page**.**  
    **IMPORTANT: This information is a mandatory requirement .**  
    nOps Single Sign-On _will not work_ if these are NOT configured.
    
    Ensure that you select the corresponding **Value** in the dropdown on the right.
    
    _The name values for the configurations are_ _case sensitive_.
    
    Enter them _exactly_ as seen. Click **Add Another** if or when you need to add additional statement rows.  
    
    ![](https://lh7-us.googleusercontent.com/ndAUAyBm2mNELdGCTLNt-cxNjv7KHo1zzbHKW1E-paCkwkYUYZsk666jNxJ1Sa6gwzZtmAqRqGLwzM3x67M8aEJP9jiMTlTkuyI9Jp1bq6Xi1DHZ_T4yutB57di4c1kZ0zvO7KKd3DYUe7pqgJgG65Q)
    
    There is information on the nOps page about setting up attribute and group attribute statements. See also [How to create a group in Okta](https://help.okta.com/en/prod/Content/Topics/users-groups-profiles/usgp-view-edit-group-attributes.htm).</br>
    **IMPORTANT:** A group name _cannot_ contain any spaces and group name must be start with **'nops-'**. You must add all potential users of nOps in the Okta group that you create.  
    You must provide the **Okta group name** on the nOps SSO dialog to enable the group and allow access.
    
6.  In the next step, select: **I'm an Okta customer adding an internal app.**
    
7.  After the app is created, click **View Setup Instructions.**  
    Copy the following items from OKTA to nOps
    



| **From Okta** | **To nOps SSO setting field** |
| --- | --- |
| Identity Provider Single Sign-on URL | SAML 2.0 Endpoint (HTTP) (singleSignOnService URL) |
| Identity Provider Issuer | Issuer URL (entityid) |
| X.509 Certificate | X.509 Certificate |

### Step 2B: From Okta, Assign Users to the nOps application ###


Only users assigned to use the nOps app you created, are enabled to use SSO when signing into nOps. You can do this step at any time and can edit users (Remove or Add) using this procedure.

1.  Under **Directory** from the main menu choose **People.**
    
2.  Click **Add Person** and enter information about the person.
    
3.  Click **Save** or **Save and Add Another** to add additional persons who will need SSO access to nOps.
    
4.  When you are done you should see a list of people in the Okta account.
    
5.  For each person you added, click **Activate** to activate the Okta SAML application.
    
6.  Go to **Applications** under the main menu and click on the Okta nOps SAML application you created.
    
7.  Under the **Assignments** field click **Assign** and find persons you want to assign to this application.
    
    Refresh your Okta application to let the changes take effect.
    
8.  You should now be able to see the nOps application you created in your Okta account.
    
9.  Clicking on the nOps app should take you to the nOps website to confirm the SSO login.
    
    nOps will send you a confirmation email.
    
10. Click the email confirmation to login using SSO.
    

## Step 3: Finish setting up nOps Configurations ##


Next navigate to the nOps SSO page. Ensure that you have copied the information from Okta.



| **From Okta** | **To nOps SSO setting field** |
| --- | --- |
| Identity Provider Single Sign-on URL | SAML 2.0 Endpoint (HTTP) (singleSignOnService URL) |
| Identity Provider Issuer | Issuer URL (entityid) |
| X.509 Certificate | X.509 Certificate |

SAML configurations for attribute and group attribute mappings. Use the following mappings:



| **Map this Attribute value** | **To this Attribute name** |
| --- | --- |
| Email | User.Email |
| First Name | User.FirstName |
| Last Name | User.LastName |
| Groups | User.groups |

### **Defining Roles and groups.** ###


**In the UserRoles/Groups section of the nOps SSO dialog:**  
Enter the name of the group you created in Okta in the **Client Admin Groups** or **Client User Groups** fields. Users of the group will have access to nOps through single sign on.

You can also check the box to specify whether the SAML group configuration can override their nOps role.

![](/tmpimg/okta-roles.png)

When you are done click **Setup SSO Configuration.**

<br/><br/>

{% include custom/series_related.html %}