---
title: OneLogin SSO Integration
keywords: sso, setup, onboarding
tags: [onboarding, sso]
sidebar: mydoc_sidebar
permalink: sso-onelogin.html
folder: Getting Started
series: [Onboarding, SSO]
weight: 6.0
---

**Steps to configure Onelogin SSO integration with nOps :** 

- **Prerequisites:** 

1. You must be logged in to your nOps and OneLogin portal.

2. You must have administrator rights in nOps to configure SSO (Single Sign-On).

3. For a smoother SSO setup, we advise opening two browser tabs. In one, log into your nOps account, and in the other, log into your OneLogin account. Copy required information between the tabs to synchronize data and enable SSO access via OneLogin.

**Step 1: Enable SSO and choose OneLogin** 

Log in to nOps as an Admin user. Navigate to **Settings**, click on **SSO** in the left panel, then click on the **Enable SSO toggle**, and select OneLogin as the **SSO type**.

![](https://lh7-us.googleusercontent.com/jWPY02150rVXIm-jscc57bqVRRAFiXNF0Rq-CDl9_XSj_V-eF-3u_63rmjPRX8WOI3ZugzGd-0E3ABON3ygjArODVEeE3n1w4czzItNlm9PFIAlNsiom1GPyFKGPwA0t1-dzS0H5R3bshrHC0WBCebo)
<br/>

**Step 2: Sign in to OneLogin and set up nOps**

1. Open a new browser tab and log in to OneLogin.

2. Go to the **Applications** page and click **Add App**.

3. Search for **SAML Custom Connector 2.0 (advanced)** to find the SAML 2.0 connector and click the icon.

4. In the **Add SAML 1.1 Test Connector** dialog, customize the **Display Name**, add an icon, and provide a description. Make sure the **Visible in portal** toggle is enabled.

5. Click **save**. Once saved, new tabs will appear in the left pane.

![](https://lh7-us.googleusercontent.com/u0KZIVc5Sc_5yZaV8CWphI9HcQ5n7lzWB7ZjD-kxNucFshLgztRSSZpt9TV1YzYSDqX_iuPAkrmgY5UbA4F7zLqnnmJpx688TJrKJ0aFn0sQl03P21LsDHgqcV7bri13urwk_7Qhps6p6LJF0iNzOx4)
<br/>

**Step 3: Go to the SSO tab from the left panel to copy the configuration details from OneLogin to nOps**

|                          |                          |
| ------------------------ | ------------------------ |
| **From OneLogin**        | **To nOps**              |
| Issuer URL (entityId)    | Issuer URL (entityId)    |
| SAML 2.0 Endpoint (HTTP) | SAML 2.0 Endpoint (HTTP) |
| X.509 Certificate        | X.509 Certificate        |

![](https://lh7-us.googleusercontent.com/iHimlIGs1Lv6nWriv8rsWsEZap4IahznSzJq_LLcBVQNmDHUXLfFLzckR6L3TuVQZeHSIOaSnRlQgLXN4Epbix2FR2JCL2b3qC6EEMDFoq-ygYSNL4CNpIuIBWMLmqPI0twnO5gBCc4F6UAgHn6z8Hs)

<br/>

## Step 4: Set up OneLogin configurations from nOps
<br/>

1. Click on the **Save** button at the top right corner after you finish 

|                          |                                                                           |
| ------------------------ | ------------------------------------------------------------------------- |
| **From nOps**            | **To OneLogin**                                                           |
| Entity Id                | Audience                                                                  |
| AssertionConsumerService | Recipient, ACS (Consumer) URL\*, ACS (Consumer) URL Validator\*  |

![](https://lh7-us.googleusercontent.com/8vzFJKtDNQPdlrZl7fwqRzVcovMdCJrsCYhuuMTBjigqZBQ7suUoErVabIESh3LZVAV10PVe9yZrR5JBVKGPVAJuKNLVc5bIcfWHqlaDjXZXkomGbjzrc26kQcxWT6GudEkHcqJBLufgaJmgQdKttdw)
<br/>

2. On the nOps platform, select the default role for the users and click on **Setup SSO Configuration**.

![](https://lh7-us.googleusercontent.com/T9xR5a80RQauqNQjceTnzBD0B-7v5h4BU-QqZreLoPvrQ00yhxWsGLhkltaD38uzZYMvE2xWm0eGck6rrGThEO_apXeSd9RPcSZJW2KhuKj0Bd1YfGqaLRlyBHVyOXh79eCU1fCioQoZBio_-tI-du8)

<br/>

## Step 5: Adding Parameters on OneLogin

Add parameters to OneLogin so that you can sync the user names and other attributes between the two applications.

Click on **Save** once you have added all parameters.

|                |            |                     |
| -------------- | ---------- | ------------------- |
| **Field Name** | **Value**  | **Include in SAML** |
| User.email     | Email      | **✅**               |
| User.FirstName | First Name | **✅**               |
| User.LastName  | Last Name  | **✅**               |

![](https://lh7-us.googleusercontent.com/bJoE2NxL08MRUrF-ah5yym7h_7-Xag2PMbLW-jTy0OvCWNHq4YYf7eg09GmbmK0qg411Bta21WiS4EUuiG5XN5IZNYU5RWXy5QLCF-SfOZdEv86W25v6sWsT7_XmDF0h5uAeqy-eOX-EKpp_D9fyT0I)


## Step 6: Adding Users on OneLogin

1. From the OneLogin app click the **Users** tab from the top toolbar.

2. Click **New User**.

3. Turn the **Active** toggle on if not.

4. Enter information about the user for the following fields: **First Name**, **Last Name**, **Email**, and **Username**.

5. Click **Save User**.

6. Navigate to the **Application** tab in the left panel and in the **Application** field click the **+** (plus) icon

7. Add the SAML 2.0 application you created (nOps SSO) earlier to grant access for this user.

8. Click on the **More Actions** dropdown and select **Send Invitation**. The user must receive the confirmation email from OneLogin, click the link to confirm, and to set a password.

9. Later when logging into OneLogin they will see the SAML 2.0 app (nOps SSO).

10. Clicking on the app directs the user to nOps which will trigger an email being sent that requires that the user confirms the email.

11. Once the confirmation is finished, the user is able to log into nOps.

![](https://lh7-us.googleusercontent.com/TSHYMTJ2-UZ35rjxH6WE_XjoXTdAiP_64jO43gsAvLiJcvBVj7pWFOdN5CQF45aCDLrKeLWstLo69bdI6qa3hkkGHsRN9KXoT6wJqhAIc8Hrf6QFYD3VurhnuVg6_m6dHxOJB-p20EZ0fDMBRm1EgsA)

![](https://lh7-us.googleusercontent.com/P22e_eNKCSV-zRdD-FbpvrrFehcx1w6G5LwwOacbqPNYFuzVBK6K2i4wB98ETgr3b5_ep0puBM28zwLRBi7w0YHg__qGYRhMdY_PV2wJDPWHuLYj8ZlF-iOP4Lu0QLIfgc1039GfcfbScSMWpPFLmIA)

![](https://lh7-us.googleusercontent.com/kcciYV7XBcIltuOlY9C_Mpnbm8hpsHS9UubpGMaH6ejIlZtd1Qcn_SdLbuPSIVLvyxw2Bbyc2QgDOhzX0Cz7NpTP3CVWVsLuviqHpqBQcD6gMcUemrFgEd71jZCaG54oEGWIseo0THkPMVL4mk8e9kI)

_Invitation email from OneLogin_

![](https://lh7-us.googleusercontent.com/o__OKLIH2yGxfPYSrk81MJ2N7e2aHy3M5D3uD6y5Y9V0NQvW0NgTWwacX3YvN1agQPo-2DRmIElKeLHtinhNEF9zxqeWg34X_bTwxhsddgi5q0QitvxH3HIQBUsHwJwRN0Zz7RYnhuOyoZ2hySON6Tk)

_nOps email requiring confirmation user SSO login_




<br/><br/>

{% include custom/series_related.html %}