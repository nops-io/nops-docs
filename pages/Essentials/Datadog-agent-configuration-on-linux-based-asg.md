---
title: DataDog Agent Configuration On your Linux based ASG
keywords: savings, recommendations, cloudwatch, essentials, rightsizing
tags: [essentials]
sidebar: mydoc_sidebar
permalink: Datadog-agent-configuration-on-linux-based-asg.html
folder: Essentials
series: [Essentials, Rightsizing]
weight: 2.0
---
### Installing the Datadog Agent for Enhanced Rightsizing Recommendation on the nOps Platform

When aiming to install the Datadog agent, it's important to note two scenarios: if a new Auto Scaling Group (ASG) template is required, begin from Step 1. If an ASG template already exists, directly proceed by copying the User Data Script for Datadog from Step 2, replacing DD\_API\_KEY with your actual key, and pasting it into your template.

Step 1: Create or Update an EC2 Launch Template for Datadog Agent

1. **EC2 Dashboard**: Go to “Services” and click on “EC2”.

2. **Launch Templates:** Under "Instances", click "Create launch template".\
3. **Update Launch Template:** Select “Launch Template” and from the action menu click “Modify template (Create new version)” 

![](https://lh7-us.googleusercontent.com/DBLd1Hn9L8w5oWTLOumXbkxycCaSPltAtSccXON_DYUsZAxmHtJ-FpLGCfhfsGv0BixiJ9tZwBN6TI6qWcVs1dHsQaYHcQxRFNVtE19lVEJJTTXVw0qWezmaA_QpSeKnC8Y_7dq9v3jPuPnf2wUPBVs)

3. **Template Details:** Provide a name and description, select an AMI if needed, and choose an instance type.

4. **IAM Role:** Choose the created IAM role in "IAM instance profile".\
Step 2: In "Advanced details", add the script to install the Datadog Agent agent:

- **User Data For Datadog Agent Installation:**

\#!/bin/bash

\# Install the Datadog Agent

DD\_API\_KEY=xxxxxxxxxxxxxxxxxxxx353792 DD\_SITE="datadoghq.com" DD\_APM\_INSTRUMENTATION\_ENABLED=host bash -c "$(curl -L https\://s3.amazonaws.com/dd-agent/scripts/install\_script\_agent7.sh)"

\# Configure Datadog to prefer IMDSv2

sudo sed -i '/site: datadoghq.com/a ec2\_prefer\_imdsv2: true' /etc/datadog-agent/datadog.yaml

\# Restart the Datadog Agent service to apply changes

sudo systemctl restart datadog-agent

![](https://lh7-us.googleusercontent.com/P5q3Oqej-jbT9n09wFPFygK8w8o24YobCOhx4TFKVlcMuk-we5ATD-0OEkOLdFEGWRXZ4LwWOcFk709o-IJaW27HfVqBtfBz5cio1JA5J_qkpy4WXNs1VcXW8XEc0C3u8pHZSuLi10l8Tl3kLrAOeiA)

Save Launch Template: Review settings and "Create launch template".

**Final Steps:**

After updating the ASG with the new launch template, any new instances launched by the ASG will have the chosen agent installed and configured. This setup enhances monitoring capabilities for these instances, allowing for better insights and optimization recommendations through nOps Rightsizing.


<br/><br/>

{% include custom/series_related.html %}