---
title: Service Control Policies
keywords: setup, onboarding
tags: [getting_started, onboarding]
sidebar: mydoc_sidebar
permalink: onboarding-service-control-policies.html
folder: GettingStarted
series: [Onboarding]
weight: 7.0
---

# Onboarding to nOps with Service Control Policies (SCP) #

As cloud security becomes more of a concern, the adoption of Control Tower and Service Control Policies (SCP) has increased substantially.  Because of their nature, `SCPs` can inhibit `nOps` from viewing necessary resource information to make cost optimization recommendations.  It can also impact visibility capabilities, such as looking at resource tags.

Our recommendation is to add an exception for the nOps role(s) to be ignored by the `SCPs`.

This can be done by adding an `ArnNotLike` statement for the `Nops-Integration` role name with a wildcard.

```json
"Resource": "*",
"Condition": {
    "StringNotEquals": {
        "aws:RequestedRegion": [
            "eu-central-1",
            "eu-west-1"
        ]
    },
    "ArnNotLike": {
        "aws:PrincipalARN": [
            "arn:aws:iam::*:role/StackSet-nOps-Integration*",
            "arn:aws:iam::*:role/Nops-Integration*"
        ]
    }
```

For additional reference regarding IAM policies required by `nOps` to make valuable recommendations, please refer to our IAM platform permissions [page](iam-minimum-platform-permissions.html).


{% include custom/series_related.html %}
