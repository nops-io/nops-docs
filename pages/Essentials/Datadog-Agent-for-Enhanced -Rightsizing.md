---
title: Datadog Agent in ASG for Enhanced Rightsizing
keywords: savings, recommendations, datadog, essentials
tags: [essentials]
sidebar: mydoc_sidebar
permalink: Datadog-Agent-in-ASG-for-Enhanced-Rightsizing.html
folder: Essentials
series: [Essentials, Storage]
weight: 2.0
---


### Installing the Datadog Agent for Enhanced Rightsizing Recommendation on the nOps Platform

When aiming to install the Datadog agent, it's important to note two scenarios: if a new Auto Scaling Group (ASG) template is required, begin from Step 1. If an ASG template already exists, directly proceed by copying the User Data Script for Datadog from Step 2, replacing DD_API_KEY with your actual key, and pasting it into your template.

Step 1: Create or Update an EC2 Launch Template for Datadog Agent

1. **EC2 Dashboard**: Go to “Services” and click on “EC2”.

2. **Launch Templates:** Under "Instances", click "Create launch template".
3. Update Launch Template: Select “Launch Template” and from the action menu click “Modify template (Create new version)” 

![](https://lh7-us.googleusercontent.com/lW1cZpXKtJzalNZ0LgeP6wds9GqQrLCiH1zd3rHSuG18QYsGsFBl0G7icaho8hFkZGynR4vtTl2TKv-8aQkDKkk_dWBvL8cUgkes8_RUTRTRomToFzy2_xnQEeDx0K19_hlivX_Xwe1Gq6tTyWXyr_8)

3. **Template Details:** Provide a name and description, select an AMI if needed, and choose an instance type.

4. **IAM Role:** Choose the created IAM role in "IAM instance profile".\
Step 2: In "Advanced details", add the script to install the Datadog Agent agent:

- **User Data For Datadog Agent Installation:**

#!/bin/bash

# Install the Datadog Agent

DD\_API\_KEY=xxxxxxxxxxxxxxxxxxxx353792 DD\_SITE="datadoghq.com" DD\_APM\_INSTRUMENTATION\_ENABLED=host bash -c "$(curl -L https\://s3.amazonaws.com/dd-agent/scripts/install\_script\_agent7.sh)"

# Configure Datadog to prefer IMDSv2

sudo sed -i '/site: datadoghq.com/a ec2\_prefer\_imdsv2: true' /etc/datadog-agent/datadog.yaml

# Restart the Datadog Agent service to apply changes

sudo systemctl restart datadog-agent

![](https://lh7-us.googleusercontent.com/6Nh2AKgqGK9HLF2JZ_lkDWa5G0DqUrunyLFKXOc48MbcUeyJA1_TcW_JYVDFKX5GVtqYfZnjBfPdNfeHZx35NPZdlFaDd_HxJQhHKTkfI7oiR2PAcQV4bf75fqWAymxmA0fOvaa1OXx_QwgU6PEnfs8)

Save Launch Template: Review settings and "Create launch template".

Final Steps:

After updating the ASG with the new launch template, any new instances launched by the ASG will have the chosen agent installed and configured. This setup enhances monitoring capabilities for these instances, allowing for better insights and optimization recommendations through nOps Rightsizing.


<br/><br/>

{% include custom/series_related.html %}