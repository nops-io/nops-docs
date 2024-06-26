---
title: SSO Integration
keywords: sso, setup, onboarding
tags: [onboarding, sso]
sidebar: mydoc_sidebar
permalink: sso-integration.html
folder: GettingStarted
series: [Onboarding, SSO]
weight: 3.0
---


# How to Integrate with SSO for nOps access #


Running a secure cloud system is very important. With the new nOps SSO feature, integrating SSO from your favorite SAML 2.0 provider is a smooth and easy process. You can currently integrate [Okta](https://www.okta.com/integrate/), [OneLogin](https://www.onelogin.com/product/sso), [Azure Active Directory (Azure AD)](https://docs.microsoft.com/en-us/azure/active-directory/) amongst others.


## Getting Started ##


To incorporate SSO in nOps, you need to configure the SSO for your SAML provider. To do that, you first need to get some credentials from your nOps dashboard.

### Your nOps Credentials ###

1.  To access your **nOps SSO credentials**, navigate to your SSO Settings Page. Go to:  
    _Organizational Settings > SSO_ if you’re using the client portal  
    or _Partner Settings > SSO_ for the partner portal_._  
    You will be prompted to enable SSO for access to the **SSO Settings** page.
    
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/sso-not-enabled.png)
    
2.  Copy the _Assertion Consumer Service and Entity ID_ values on the SSO Settings page and paste them into your SAML provider’s SSO configuration settings.
    
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/sso-integration-details.png)
    
3.  Next you need to map some defined attributes. This should be done using the exact values as described. These attributes are called “Parameters” in OneLogin.
    
    | **Map this Attribute value** | **To this Attribute name** |
    | --- | --- |
    | Email | User.Email |
    | First Name | User.FirstName |
    | Last Name | User.LastName |
    | Groups | User.groups |

When you are done, you will be provided setup instructions which you will then use to configure SSO on nOps.

### Configuring SSO on nOps ###


After setting up SSO with your SAML provider, configure SSO on nOps.

To do that, you need some key credentials from Okta or OneLogin (i.e. your provider). They are:

* Issuer URL (entityId)
    
* SAML 2.0 Endpoint (HTTP) (singleSignOnService: URL)
    
* X.509 Certificate
    

Copy these values and paste them in their respective input fields on the nOps SSO settings page shown below.

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/sso-details-filled.png)

### Assigning Users ###

After completing these steps, you can add existing users to your application. New users will need to complete a one-time email activation in order to have SSO enabled for them.

## Additional Features ##

nOps has some new features that you can activate for your SSO integration.

### Enable SSO Login ###


When you enable the **Enable SSO Login** toggle shown below, users will be redirected to the SSO login for authentication the next time they try to sign in and will only need to provide their email to Sign in to nOps.

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/sso-enable-toggle.png)

Leaving this feature disabled will require users to log in with their current login password credentials. However, this is only possible for users who went through the nOps sign-up process.

### Enforce SSO login ###

To enforce SSO login for **all** users, you must specify a domain in the input box and also select the **Enforce SSO login for all domain users** checkbox shown below.

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/sso-enforce.png)

Users coming from the specified domain address, _must_ use the SSO Login process to sign in or they will be denied access.

If you however want to login from another domain name, you can copy the value shown in the _Shareable Link for IDP Login_ and sign in using that.

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/sso-shareable.png)

### Setting User Roles ###


This feature allows you to choose a default role for users. You can choose between:  
\- _client-member_ and _client-admin_ if you are using the Client nOps portal

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/sso-client-role-options.png)

OR  
\- _partner-member and partner-admin_ if you are using the Partners nOps portal.

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/sso-partner-role-options.png)

**For the Partner portals**: the _partner-admin role_ can send invitations, configure SSO and access the partners’ clients. A _partner-member_ role has limited access only to clients.

**For the Client portal:** the _client-admin_ role has access to all available options including SSO while the _client-member role_ has **no access privileges** to the Settings pages.

### Control your SSO user groups ###
----------------------------

You can also **control your SSO user groups** by ​​setting an nOps role based on the SAML group. This feature is currently only available for Okta.

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/sso-assign-groups.png)

To enable this feature, you need to specify at least one value for admin and user groups.

In addition, you can also select the **_Allow SAML Group Configuration to Override nOps Role_** checkbox. This will give preference to _nOps defined roles_ over that of your specified provider’s roles.


### Update or Delete SSO Configuration ###
----------------------------------

Lastly, you can update your SSO configuration or delete it entirely.

![](https://nops-help-site-assets.s3.amazonaws.com/images/gettingstarted/sso-save-or-delete.png)

{% include warning.html content="Deleting your SSO integration is irreversible. You cannot undo the deletion."%}


<br/><br/>

{% include custom/series_related.html %}