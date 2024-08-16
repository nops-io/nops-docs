---
title: Cost Analysis With Role-based Access Control
keywords: cost reporting, cost history, cost allocation, business context, cost analysis, rbac
tags: [cost_visibility, bc]
sidebar: mydoc_sidebar
permalink: business-contexts-rbac.html
folder: BusinessContexts
series: [bc]
weight: 1.0
---

## Role-Based Access Control for Business Contexts ##

Role-Based Access Control (RBAC) is designed to provide precise control over who can access specific cost data within an organization. This system ensures that team members only see the information relevant to their roles, enhancing both security and efficiency.

## Key Benefits of RBAC in Showback ##

1. **Controlled Access**: By implementing RBAC, you can restrict access to sensitive cost information, ensuring that members only see the data pertinent to their responsibilities. Team members benefit from quick access to the precise information they need without navigating through irrelevant data.

2. **Enhanced Security**: Limiting access reduces the risk of unauthorized data exposure, helping to maintain a higher security standard.

3. **Efficient User Management**: With RBAC, managing user permissions is streamlined. Roles are defined based on job functions, and users are assigned to these roles, automatically granting them the necessary permissions.

4. **Improved Accountability**: With role-specific access, team members can take ownership of their cloud usage and costs, promoting a culture of accountability and cost awareness.

## How RBAC works: Flexible and granular access control ##

Roles define the data access permissions possessed by each user. Admins can modify and assign specific access through default or custom roles. RBAC policy is flexible and aligns object-level permissions in a similar manner to AWS Identity and Access Management (IAM). 

Using RBAC, you can control access to the following policies:

| Term | Definition |
| --- | -- |
| Showback Values  | This policy grants users access to specific Showback values based on their roles. It ensures that users can view only the cost data pertinent to their responsibilities, promoting both security and relevance in the data they access.               |
| Cost Analysis Page Default View | This policy controls the default view settings on the Cost Analysis page. It ensures that users see a tailored, role-specific view of cost data when they access this page, streamlining their ability to analyze and manage cloud costs efficiently. |

## Get Started with RBAC: Summary ##

To start using the Role-Based Access Control (RBAC) feature for Showback, follow these basic steps (described in more detail with screenshots below). 

1. **Navigate to the Feature:**

   - Go to [nOps](https://app.nops.io).

2. **Access Cost Allocation:**

   - Navigate to **Business Context** **→** **Cost Allocation**.

3. **Choose the Showback:**

   - Select the specific Showback you want to manage.

4. **Modify Access Control:**

   - Go to the detail page of the selected Showback.

   - Select the Showback values you want to include in the policy.

   - Click on **Modify Access Control**.

5. **Create the Policy:**

   - In the popup that appears, enter a **Policy Name**.

   - Select the checkbox if you want to grant access to the default view.

   - Select the members who should have access to these Showback values.

   - Save the policy to finalize it.

6. **Manage Policies:**

   - You can see the list of all policies on this page: [Team Members - Policies](https://app.nops.io/v3/settings?tab=Team+Members\&subTab=Policies).


## Get Started with RBAC: A Complete Guide ##

### Navigate to the Cost Allocation in Business Contexts Plus ###

- Go to [nOps](https://app.nops.io)

- Navigate to **Business Context** **→** **Cost Allocation**.
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfczll8UWrmUgiJ5ymW_cLGyYBZf4yMOzwpglr5TG_NA_k5RSYdt8Up5zhZCUU8FH4-9qBU0ceDRleqGmo5eymPk1sOnfQz7Pj77iJqW4rXaS9JJWu43deQomQPc5JCLdrLTrXQAXg_LtGdJmKThDopCk86?key=RSBpj00gWnCgWKgVsODaPw)


### Select the Showback ###

- Choose the specific Showback you want to manage (e.g., "Cost Center").

- Go to the detail page of the selected Showback.
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfGzcm5cWXwcrKBlQQxiTI3Aycehk7m8VuXhrlmEIc87y0pFLCrcyY7RFIAbyt5iG_2bH4eF5JX-JqRsR61TWF6EBuVofDJlklk0b9Bz45hRqr_Us3B-8DCD_2Cg7sguakE6x31ceUkdEYoo5x-6xz1i-P9?key=RSBpj00gWnCgWKgVsODaPw)


### Modify Access Control ###

- Select the Showback values you want to include in the policy (e.g., 101, 102, 103).

- Click **Modify Access Control**.
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeLoZkXs-OPKxHGK60iiqpdhnWrHM9HB_eEheugoNczFSEHGxnSmOWc7CvMVEE2AbAc-xjrsPK_381sIMFBRHOPt43sBvTzU7hgGq2rmi0Aaux-OHsPals7w51mbFTEn2fTOVDqmOK5Snp5yV1P6U5yegBJ?key=RSBpj00gWnCgWKgVsODaPw)


### Create and Configure the Policy ###

- In the popup that appears, enter a **Policy Name**.

- Select the checkbox to grant access to the default view.

- Choose the members to receive access.

- Save the policy to finalize it.
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXd2uraB4mzjFxEAi4tv2eoLiz6-_wFbmr8VYK54k00C_TwOJ8vNB8VXjkEDBhlOO8IBRzII5lNYeeTOMf3okCDLnBHC98vtiLopTb0SNGTuGgU4gw5Q2LVKK6Jypfp_sttPgi_uQmQ-oO7SKm_i_D1BUAc?key=RSBpj00gWnCgWKgVsODaPw)


### Viewing Policies ##

- You can view and manage all policies on this page:[ Team Members - Policies](https://app.nops.io/v3/settings?tab=Team+Members\&subTab=Policies).
![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdhF8Uvb3bzBlpbw4TrGVKlfDzNDaTMuJQFVUBsub3fJEORIWwLXhWNS_-yo4UikOVHkJWnXCqNhb3q4smlzeeupkdmudS5cEFkiRhkBKWJ3h1_bTxoEeGWFg1YF2zI3FTV0rFzbzXYerm6dyEB0-fkO6HV?key=RSBpj00gWnCgWKgVsODaPw)


## Use Cases & Examples ##

Let’s go through some real-world examples to illustrate how RBAC in Business Contexts Plus works. 


### Scenario 1: Member’s Access Without Default View and Only One Showback Access ##

**Cost Allocation:**

- When the member logs in and navigates to the Cost Allocation section, they will see only the Showback values (101, 102, 103) specified in their policy.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfIixqMD8l_hGTndBH-LTTfA4eXcW3eVypC1hitxTMDyJpC8-pLOMP6_DplbLu4EUg11MwGugRzFW_MymkXnr1MQc95WpjGpgBxviNWej0Ev-2FUgdT2aYtCKVgeSHbukAS_NVvVLT8L_qw7ocFVJlFJ3mf?key=RSBpj00gWnCgWKgVsODaPw)

**Cost Analysis** 

- Since the member does not have access to a default view, they will only see the Showback view without the default view option.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeMAHqeGRdgwlk7z5tEWwpTCyt9jfPIGQCjZWB5leNuABXfpFSPLT9J_4uEP9zGo8VDwGx28T_PNQjIdwcYWVHJ11o9SSBEs7y2LxIMuWKEbbc5kprXoG84Xml4bNbe2QoAG_Xs85cvZjx6PirglPdqUi5x?key=RSBpj00gWnCgWKgVsODaPw)



### Scenario 2: Member’s Access Without Default View and Multiple Showback Access ###

**Cost Analysis:**

- Without default view access, the member will see a dropdown to change the Showback view, but they will not have the default view option available.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXczWoNesNyIqbd1ib5UnjFQNEfcEFzWpf1iKoxMKrtaNjbOsrtVDYLGnVW5fcCk0bqagJQrkZ5wDbm8lr2vHKLxrzMEoN5tIIO0NZ9EuirGOMTBcS3Fi-VlpXJ46RuUjCg6SzDXecOrAc8o42GTuMR_LgzS?key=RSBpj00gWnCgWKgVsODaPw)


### Scenario 3: Member’s Access With Default View ###

**Cost Analysis:**

- With default view access, the member will see the default view on the Cost Analysis page and will have the option to switch from the default view to the Showback view.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXeROvS0gsVwJ7_6T01ltrxOfifbZwsWz6P1xedikDrTenG7WKIwyt7YUGEexWptqDiiFqIPWk_QpLRBDqdF1JLRJDkDqftN8iW979YUOHHMMBpioOuUqOd_5WG_XSj7ZOfjz96Nwo_34aMEm14UmPkLFLcB?key=RSBpj00gWnCgWKgVsODaPw)


### Scenario 4: Member Without Any Showback Access ###

**Cost Allocation:**

- The member will have no access to the Cost Allocation section.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfIcDCskIw6LPLGiHSUfTPj0sae7cCvhdCO1x5xdIznNFEI2Cr0nrN2xV-v0LE4ewjYoyawW9SX6nZtNAvjWTU_y75NcfiNXpM-EaJbcMYD69dz9B6ZVZX0vf48qq-a94JoY2GiPk9PJ8swETvWsyuwjJB3?key=RSBpj00gWnCgWKgVsODaPw)

**Cost Analysis:**

- The member will not be able to access the Cost Analysis page.

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcg2DwDp4MZ-xRSvYZbDz-tSogLuMFg2tH0HPm4DFTU1KRTNO7Cp-iBnXBQCgch3FmS4kI22jMqsCpGYwh9Tul_czA8GGuCKzawGmZCH4M-Pv1k3yr7IiSPPaag-JWQ3sglMvTsSIX6rGlskxlC1vAASFE?key=RSBpj00gWnCgWKgVsODaPw)


{% include note.html content="When creating a policy, if the user selects the **“Select All”** option (meaning all Showback values), any new custom Showback values added to that Showback after the policy is created will also be included in the policy. This ensures that the policy remains up-to-date with any changes or additions to the Showback values.

For instance, if you initially select all existing Showback values and later add new values to the Showback, these new values will automatically be covered by the policy, eliminating the need for manual updates.
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcRXNo8IqX49IuFGE9jhD1HCs-Fj8kgxlNBks44fjMVvuoLGizyY0zLbMI6DWoYgNUtGB0OGa_LbDAesR5JmoeApvD6gTFjLbmEvOKq7O_MQQUpRMwJsO6Oq6E8KaPmF0lJ6WjURsIR0IX161wLPYu9QiGy?key=RSBpj00gWnCgWKgVsODaPw) "%}


<br/><br/>

{% include custom/series_related.html %}