---
title: Compute Copilot Spot Instance Reliability SLA
keywords: savings, recommendations, sharesave, spot, cost copilot, asg, eks, nks
tags: [copilot]
sidebar: mydoc_sidebar
permalink: copilot-service-sla.html
folder: Copilot
series: [Copilot]
weight: 3.0
---

# Effective Date: September 7th, 2023 #

## Introducing the Compute Copilot Spot Instance Reliability SLA ##

We are excited to announce a new and unique Service Level Agreement (SLA) that sets us apart in the market and underscores our commitment to your success with our Compute Copilot engine.

### What is it? ###

Our new SLA focuses on ensuring the reliability of Spot Instances within our Compute Copilot product for customers who have connected their Amazon Elastic Kubernetes Service (EKS), AutoScaling Group, or Databricks job to our service. ***You can run your workload on spot with the same confidence as AWS on-demand.*** 

### What does it guarantee? ###

In simple terms, if we place your workload on an instance that receives a termination event, and we fail to replace that instance before termination, we will cover the terminated AWS instance's hourly usage fee for up to the entire month of the termination. Yes, you read that right – we've got you covered!

### Why is this different? ###

This SLA goes beyond the ordinary and demonstrates our unwavering commitment to your success and satisfaction. We understand how critical Spot Instances are for your workloads, and we want to ensure you have peace of mind.

### Do I need to report an incident? ###

Never. Any credit will be automatically applied as part of our monthly billing cycle. If, by any chance, you think we missed a credit, simply report the incident to our support team within 72 hours of final monthly billing, and we'll take care of the rest.

### When does it take effect? ###
The Compute Copilot Spot Instance Reliability SLA is effective from September 6th, and we can't wait to show you the difference it makes in your experience with our service.


### Are there any other considerations? ###

- Does not apply in the event of an Amazon service outage impacting the region in the termination.
- You must diversify by selecting at least five instance families as part of your node template configuration.
- You must have at least three different availability zones included in your cluster.

At nOps, we are always striving for excellence, and this SLA is another step in that direction. If you have any questions or need further clarification, please don't hesitate to reach out to our support team.


{% include custom/series_related.html %}
