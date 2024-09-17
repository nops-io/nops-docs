---
title: The nOps Essentials Storage API Guide
keywords: essentials, storage, developer, api
tags: [developer, essentials, storage]
sidebar: mydoc_sidebar
permalink: essentials-storage-api.html
folder: Developer
series: [Essentials API]
weight: 3.0
---

## Authentication ##

The first step is to authenticate to the nOps API. This [article](https://help.nops.io/developer-getting-started.html) provides more detailed information on the API authentication process.

Next, we proceed to work with the API.

## Essentials Storage Recommendations ##

### GP2 to GP3 Recommendations ###

The following request provides you a summary about **GP2 to GP3 Recommendations**:

    GET https://app.nops.io/svc/k8s_cost/recommendations/essentials/storage/gp2/summary/

This request can accept the following query parameters (all are optional):

- `project_id`: Filter - comma separated list of nOps AWS Project IDs. Default: all projects.

The following request lists your **GP2 to GP3 Recommendations**:

    GET https://app.nops.io/svc/k8s_cost/recommendations/essentials/storage/gp2/

This request can accept the following query parameters (all are optional):

- `page_number`: Page number. Default: `1`.
- `page_size`: Page size. Default: `10`.
- `sort_type`: Either `asc` or `desc` - the result will be ordered by savings in corresponding way. Default: `desc`.
- `project_id`: Filter - comma separated list of nOps AWS Project IDs. Default: all projects.

To migrate your resources to GP3, you need to send a request to the following endpoint:

    POST https://app.nops.io/svc/notifications/scheduler/nops_scheduler

In the payload of the request, specify the following fields:

- `actions`: an array with one object consisting of the following fields:
    - `action`: `ebs_migration`;
    - `action_type`: should be `onetime` for Storage;
    - `day_of_week`: an integer from 0 to 6 which represents day of the week (0 is Sunday, 1 is Monday and so on);
    - `hour`: an integer from 0 to 23;
    - `minute`: an integer from 0 to 59;
- `resources`: an array with Storage Resources consisting of the following fields:
    - `item_type`: `ebs`;
    - `resource_id`: AWS EBS resource ID;
    - `resource_name` (optional): AWS EBS resource name or ID;
    - `resource_arn`: AWS resource ARN;
    - `region`: AWS region;
    - `resource_details`: empty object;
    - `price_per_unit`: current resource price per hour;
- `project`: nOps project ID;
- `account_number`: AWS Account ID;
- `eventbridge_configuration`: AWS EventBridge Configuration ID for nOps;
- `scheduler_for`: should be `essentials__storage` for Storage;
- `description`: Description;

Here is an example of a payload:

<code>{
    "project": "11111",
    "account_number": "123123123123",
    "eventbridge_configuration": "4288edde-fe34-4d6f-b5cb-e7ca9ce823ed",
    "scheduler_for": "essentials__storage",
    "description": "Migrate my resources!",
    "resources": [
        {
            "item_type": "ebs",
            "resource_id": "vol-0000000000000000",
            "resource_name": "My-Test-Volume-1",
            "resource_arn": "vol-0000000000000000",
            "region": "us-west-2",
            "resource_details": {},
            "price_per_unit": 100
        },
        {
            "item_type": "ebs",
            "resource_id": "vol-0000000000000001",
            "resource_name": "My-Test-Volume-2",
            "resource_arn": "vol-0000000000000001",
            "region": "us-west-1",
            "resource_details": {},
            "price_per_unit": 100
        }
    ],
    "actions": [
        {
            "action": "ebs_migration",
            "action_type": "onetime",
            "day_of_week": 4,
            "hour": 19,
            "minute": 15
        }
    ]
}</code>

### Snapshot Recommendations ###

The following request provides you a summary about **Snapshot Recommendations**:

    GET https://app.nops.io/svc/k8s_cost/recommendations/essentials/storage/snapshots/summary/

This request can accept the following query parameters (all are optional):

- `project_id`: Filter - comma separated list of nOps AWS Project IDs. Default: all projects.

The following request lists your **Snapshot Recommendations**:

    GET https://app.nops.io/svc/k8s_cost/recommendations/essentials/storage/snapshots/

This request can accept the following query parameters (all are optional):

- `page_number`: Page number. Default: `1`.
- `page_size`: Page size. Default: `10`.
- `sort_type`: Either `asc` or `desc` - the result will be ordered by savings in corresponding way. Default: `desc`.
- `project_id`: Filter - comma separated list of nOps AWS Project IDs. Default: all projects.

To archive your snapshots, you need to send a request to the following endpoint:

    POST https://app.nops.io/svc/notifications/scheduler/nops_scheduler

In the payload of the request, specify the following fields:

- `actions`: an array with one object consisting of the following fields:
    - `action`: `archive_snapshot`;
    - `action_type`: should be `onetime` for Storage;
    - `day_of_week`: an integer from 0 to 6 which represents day of the week (0 is Sunday, 1 is Monday and so on);
    - `hour`: an integer from 0 to 23;
    - `minute`: an integer from 0 to 59;
- `resources`: an array with Storage Resources consisting of the following fields:
    - `item_type`: `snapshot`;
    - `resource_id`: AWS EBS Snapshot resource ID;
    - `resource_name` (optional): AWS EBS Snapshot resource name or ID;
    - `resource_arn`: AWS resource ARN;
    - `region`: AWS region;
    - `resource_details`: empty object;
    - `price_per_unit`: current resource price per hour;
- `project`: nOps project ID;
- `account_number`: AWS Account ID;
- `eventbridge_configuration`: AWS EventBridge Configuration ID for nOps;
- `scheduler_for`: should be `essentials__storage` for Storage;
- `description`: Description;

Here is an example of a payload:

<code>{
    "project": "11111",
    "account_number": "123123123123",
    "eventbridge_configuration": "4288edde-fe34-4d6f-b5cb-e7ca9ce823ed",
    "scheduler_for": "essentials__storage",
    "description": "Archive my snapshots!",
    "resources": [
        {
            "item_type": "snapshot",
            "resource_id": "snap-0000000000000000",
            "resource_name": "My-Test-Snapshot-1",
            "resource_arn": "snap-0000000000000000",
            "region": "us-west-2",
            "resource_details": {},
            "price_per_unit": 100
        },
        {
            "item_type": "snapshot",
            "resource_id": "snap-0000000000000001",
            "resource_name": "My-Test-Snapshot-2",
            "resource_arn": "snap-0000000000000001",
            "region": "us-west-1",
            "resource_details": {},
            "price_per_unit": 100
        }
    ],
    "actions": [
        {
            "action": "archive_snapshot",
            "action_type": "onetime",
            "day_of_week": 4,
            "hour": 19,
            "minute": 15
        }
    ]
}</code>

## Getting your scheduled or triggered actions ##

You can fetch details about all your scheduled or triggered actions with the following:

    GET https://app.nops.io/svc/notifications/scheduler/nops_scheduler

This request requires the following query parameters:

- `api_key`: Your nOps API key.
- `scheduler_for`: With `essentials__storage` value it means you want to get Storage only.
