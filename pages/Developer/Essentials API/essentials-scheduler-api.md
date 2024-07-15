---
title: The nOps Essentials Scheduler API Guide
keywords: essentials, scheduler, developer, api
tags: [developer, essentials, scheduler]
sidebar: mydoc_sidebar
permalink: essentials-scheduler-api.html
folder: Developer
series: [Essentials API]
weight: 3.0
---

## What is nOps Essentials Scheduler? ##

**nOps Essentials Scheduler** automatically pauses specified unused cloud resources, such as EC2 instances, EBS volumes, RDS instances and EKS clusters. It also can work by creating tag-based schedules. For example, an organization might create a Scheduler that pauses all EC2 instances with the generated tag during off-peak hours.

## Why use nOps Essentials Scheduler? ##

- **Cost optimization.** Scheduling can save organizations a significant amount of money on Dev and QA costs, especially during off-peak hours.
- **Time savings.** Lack of time and resources is a key blocker preventing engineers from reducing cloud spend — nOps Essentials Scheduler makes scheduling simpler and faster.
- **Automation**. Our AI is constantly learning your usage patterns for greater savings.

***

## Authentication ##

The first step is to authenticate to the nOps API. This [article](https://help.nops.io/developer-getting-started.html) provides more detailed information on the API authentication process.

Next, we proceed to work with the API.

## Scheduler Opportunity Summary ##

Let's start by getting information about the opportunity you can get with our recommendations. The API returns the amount that your company can save if it accepts all the recommendations of nOps Essentials. To get it, you need to send the following request:

    GET https://app.nops.io/svc/k8s_cost/essentials/recommendations/scheduler/all/summary/

This request can accept the following query parameters (all are optional):

- `start_date`: Start date for needed period. Format: `YYYY-MM-DD`. Default: today + 30 days.
- `end_date`: End date for needed period. Format: `YYYY-MM-DD`. Default: today.
- `project_ids`: Comma separated nOps AWS Project IDs. Default: all projects.

> Note: the amount shown here is for Workload-Based recommendations only. You can get detailed information on Utilization-Based recommendations by following the [instructions below](#utilization-based-recommendations).

## Essentials Scheduler Recommendations ##

Essentials provides a lot of recommendations based on resources' metrics: Utilization and Workload Based.

> Note: if you don't have Essentials Premium Package - all your results will be limited in 3 recommendations.

### Utilization-Based Recommendations ###

The following request provides you a summary about **Utilization-Based Recommendations**:

    GET https://app.nops.io/svc/k8s_cost/recommendations/essentials/scheduler/utilization/summary/

This request can accept the following query parameters (all are optional):

- `project_id`: Filter - comma separated list of nOps AWS Project IDs. Default: all projects.
 
The following request lists your **Utilization-Based Recommendations**:

    GET https://app.nops.io/svc/k8s_cost/recommendations/essentials/scheduler/utilization/

This request can accept the following query parameters (all are optional):

- `page_number`: Page number. Default: `1`.
- `page_size`: Page size. Default: `10`.
- `sort_type`: Either `asc` or `desc` - the result will be ordered by savings in corresponding way. Default: `desc`.
- `project_id`: Filter - comma separated list of nOps AWS Project IDs. Default: all projects.

### Workload-Based Recommendations ###

The following request provides you a summary about **Workload-Based Recommendations**:

    GET https://app.nops.io/svc/k8s_cost/recommendations/essentials/scheduler/workload/summary/

This request can accept the following query parameters (all are optional):

- `project_id`: Filter - comma separated list of nOps AWS Project IDs. Default: all projects.
- `environment`: Filter - comma separated list of Environments. Default: all environments.
 
The following request lists your **Workload-Based Recommendations**

    GET https://app.nops.io/svc/k8s_cost/recommendations/essentials/scheduler/workload/

This request can accept the following query parameters (all are optional):

- `page_number`: Page number. Default: `1`.
- `page_size`: Page size. Default: `10`.
- `sort_type`: Either `asc` or `desc` - the result will be ordered by savings in corresponding way. Default: `desc`.
- `project_id`: Filter - comma separated list of nOps AWS Project IDs. Default: all projects.
- `environment`: Filter - comma separated list of Environments. Default: all environments.

## Create your Essentials Scheduler ##

You can use any recommendation from the suggested by nOps or create your own Scheduler with resources whose lifecycle you want to control. When using a recommendation, creation becomes much easier because you only need to form a request from the information provided in the recommendation.

To create a Scheduler, you need to send a request to the following endpoint:

    POST https://app.nops.io/svc/notifications/scheduler/nops_scheduler

In the payload of the request, specify the following fields:

- `actions`: an array with Scheduler Actions consisting of the following fields:
    - `action`: `start` / `stop`;
    - `action_type`: should be `selected_day_of_week` for Scheduler;
    - `day_of_week`: an integer from 0 to 6 which represents day of the week (0 is Sunday, 1 is Monday and so on);
    - `hour`: an integer from 0 to 23;
    - `minute`: an integer from 0 to 59;
- `resources`: an array with Scheduler Resources consisting of the following fields:
    - `item_type`: `ec2` / `rds` / `rds_cluster` / `autoscaling_groups` / `eks_nodegroups`;
    - `resource_id`: AWS resource ARN. For AWS EC2 it should be instance ID;
    - `resource_name` (optional): AWS resource name;
    - `resource_arn`: AWS resource ARN;
    - `region`: AWS region;
    - `resource_details`: empty object for EC2 / RDS, scaling config for ASG or EKS with the following fields:
        - `MinSize`: min size;
        - `MaxSize`: max size;
        - `DesiredCapacity`: desired capacity;
    - `price_per_unit`: current resource price per hour;
- `project`: nOps project ID;
- `account_number`: AWS Account ID;
- `eventbridge_configuration`: AWS EventBridge Configuration ID for nOps;
<!-- - `eventbridge_configuration`: AWS EventBridge Configuration ID for nOps. Check this [article](https://help.nops.io/create-and-configure-eventbridge.html) for more details; -->
- `scheduler_for`: should be `scheduler_start_stop` for Scheduler;
- `description`: Scheduler description;
- `hours_down`: the number of hours during which resources will be down for 4 weeks (up to 167);

Here is an example of a payload:

<code>{
    "project": "11111",
    "account_number": "123123123123",
    "eventbridge_configuration": "4288edde-fe34-4d6f-b5cb-e7ca9ce823ed",
    "scheduler_for": "scheduler_start_stop",
    "description": "Stopping test EC2 resources!",
    "hours_down": 80,
    "resources": [
        {
            "item_type": "ec2",
            "resource_id": "i-0000000000000000",
            "resource_name": "My-Test-Instance",
            "resource_arn": "i-0000000000000000",
            "region": "us-west-2",
            "resource_details": {},
            "price_per_unit": 100
        },
        {
            "item_type": "autoscaling_groups",
            "resource_id": "arn:aws:autoscaling:us-west-2:123123123123:autoScalingGroup:4288edde-fe34-4d6f-b5cb-e7ca9ce823e1:autoScalingGroupName/asg-name",
            "resource_name": "asg-name",
            "resource_arn": "arn:aws:autoscaling:us-west-2:123123123123:autoScalingGroup:4288edde-fe34-4d6f-b5cb-e7ca9ce823e1:autoScalingGroupName/asg-name",
            "region": "us-west-2",
            "resource_details": {
                "MinSize": 1,
                "MaxSize": 3,
                "DesiredCapacity": 2
            },
            "price_per_unit": 50
        }
    ],
    "actions": [
        {
            "action": "stop",
            "action_type": "selected_day_of_week",
            "day_of_week": 4,
            "hour": 19,
            "minute": 15
        },
        {
            "action": "start",
            "action_type": "selected_day_of_week",
            "day_of_week": 0,
            "hour": 8,
            "minute": 0
        }
    ]
}</code>

> Note: If you create a Scheduler by passing an empty `resources` field, nOps Essentials will generate custom tag keys and tag values for you in the `scheduled_by_tag_key` and `scheduled_by_tag_value` fields, respectively. You can add this tag to your AWS resources and after 24 hours our system will automatically add those resources to Scheduler and you will see a special label on UI. We recommend using Scheduler by Tag for DEV and QA resources, where the schedule grid of the resources directly depends on the work schedule of the engineering team. It's also convenient because you don't have to add or remove resources on the nOps side every time. Add and remove tags from relevant resources in AWS and nOps will do everything else by itself.

## Getting your nOps Essentials Schedulers ##

You can fetch details about all existing Schedulers with the following:

    GET https://app.nops.io/svc/notifications/scheduler/nops_scheduler

This request requires the following query parameters:

- `api_key`: Your nOps API key.
- `scheduler_for`: With `scheduler_start_stop` value it means you want to get Schedulers only.

## Disabling/Enabling Scheduler ##

As an additional option, you can disable any Scheduler. This is useful if you want the Scheduler to do nothing with your resources some time. So instead of removing the scheduler, you only need to disable it.

To deactivate the Scheduler, you need to send the following:

    POST https://app.nops.io/svc/notifications/scheduler/{id}/disable

In response, you will receive a 202 status code if the service successfully disabled the Scheduler.

To enable the Scheduler, send the following request:

    POST https://app.nops.io/svc/notifications/scheduler/{id}/enable

## Triggering actions ##

Sometimes you may need to start or stop your resources, deviating from the schedule. Manual start or stop is possible using the following request:

    POST https://app.nops.io/svc/notifications/scheduler/{id}/trigger

In the payload of the request, specify the following field:

- `action_name`: `start` or `stop`.
