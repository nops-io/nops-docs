---
title: Map Migration API Documentation
keywords: migration, MAP, public, map tracking, map credits, developer, api
tags: [map]
sidebar: mydoc_sidebar
permalink: map-migration-api.html
folder: Developer
series: [MAP API]
weight: 3.0
---


## Introduction

The nOps Map Migration API provides endpoints to manage and retrieve information about MAP Migration projects signed by customer with AWS. This includes listing all current MAP projects, retrieving details of a specific project, listing products within a project, and listing resources within a project.

Try it in our [Public Swagger](https://app.nops.io/public_swagger/)

## Endpoints

### 1. List All Map Migration Projects

- **URL**: `/c/v3/map-migration/`
- **Method**: `GET`
- **Description**: Lists all MAP Migration projects for the current client.

#### Request Parameters

None.

#### Response Format

The response is a JSON array containing a list of map migration projects. Each project has the following fields:

- `id`: The ID of the MAP Migration project. This ID is nOps specific. nOps assigns a unique ID to each project you onboard to the MAP Dashboard.
- `on_prem_non_commercial_db_entity`: Specific incentives related to on-premises non-commercial database.
- `on_prem_commercial_db_entity`: Specific incentives related to on-premises commercial database.
- `ec2_non_commercial_db_entity`: Specific incentives related to EC2 non-commercial database.
- `ec2_commercial_db_entity`: Specific incentives related to EC2 commercial database.
- `sap_entity`: Specific incentives related to SAP.
- `baseline_s3_entity`: Specific incentives related to S3.
- `speciality_services_entity`: Specific incentives related to speciality services.
- `private_equity_entity`: Specific incentives related to Private Equity.
- `tagged_resources`: Information about tagged resources, including resource count and cost.
- `untagged_resources`: Information about untagged resources, including resource count and cost.
- `credits`: Information about received credits, including records of credit details.
- `client`: The client associated with the MAP project. Name, website and the client ID, which is another nOps specific value that gets generated and assigned to each nOps platform client at the time of account creation in nOps. You will find your assigned Client ID at the top right corner of nOps platform screen under your profile name.
- `created`: The creation date of the project.
- `modified`: The last update date of the project.
- `name`: The name of the MAP project. This is also something you specify while onboarding the MAP project in nOps MAP Dashboard and it can be renamed later as well.
- `migration_id`: The migration ID is identified by the project number in the contract, of the form MPExxxxx (where xxxxx are digits) or a 10-digit identifier such as ABCLMNOPYZ. If you are not sure where to find your MAP Project ID, refer to the top of the first page of your MAP contract.
- `tag_key`: Must be the exact value string from your AWS contract. This value should be mig followed by the 5 or 10-digit MAP ID. If it’s an older MAP contract, you may also use the server-id (typically d- followed by 9 digits).
- `server_id`: The server ID, as referred above.
- `credit_percent`: Prefilled for you to the typical 25%, however you may edit this when creating a project in nOps if your contract terms state a different discount for standard MAP resources.
- `workload_pk`: Primary key of a workload (deprecated).
- `date_start`: The start date of the migration (per your MAP contract terms from AWS).
- `date_end`: The end date of the migration (per your MAP contract terms from AWS).
- `migration_period`: The migration period in months.
- `windows_optimization`: Indicates if Windows optimization is enabled.
- `map_credit_cap`: The MAP credit cap (default is 1.000.000USD).

---

### 2. Retrieve a Specific Map Migration Project

- **URL**: `/c/v3/map-migration/{id}/`
- **Method**: `GET`
- **Description**: Retrieves details of a specific map migration project.

#### Request Parameters

- **Path Parameter**:
  - `id` (required): The ID of the MAP Migration project. This ID is nOps specific. nOps assigns a unique ID to each project you onboard to the MAP Dashboard.

#### Response Format

The response is a JSON object like the one above

### 3. List Products in a Map Migration Project

- **URL**: `/c/v3/map-migration/{id}/products/`
- **Method**: `GET`
- **Description**: Lists all the products in a map migration project.

#### Request Parameters

- **Path Parameter**:
  - `id` (required): The ID of the MAP Migration project. This ID is nOps specific. nOps assigns a unique ID to each project you onboard to the MAP Dashboard.

#### Response Format

The response is a JSON array containing a list of products in the map migration project. Each product has the following fields:

- `product_product_name`: The name of the product (a product here refers to an AWS Service, example: Amazon Elastic Compute Cloud etc.)
- `resources_count`: The amount of resources of that product (using hll approximation).
- `cost`: The total cost of all the resources.

##### Example Response

```json
[
    {
        "product_product_name": "Amazon Simple Storage Service",
        "resources_count": 123.0,
        "cost": 123.0
    },
    {
        "product_product_name": "Amazon Relational Database Service",
        "resources_count": 123.0,
        "cost": 123.0
    }
]
```

### 4. List Resources in a Map Migration Project

- **URL**: `/c/v3/map-migration/{id}/resources/`
- **Method**: `GET`
- **Description**: Lists the resources in a map migration project.

#### Request Parameters

- **Path Parameter**:
  - `id` (required): The ID of the MAP Migration project. This ID is nOps specific. nOps assigns a unique ID to each project you onboard to the MAP Dashboard.

- **Query Parameters**:
  - `migration_entity` (optional): Filter resources by migration entity.
  - `tagged` (optional): Filter resources by their tagged status. True = tagged.
  - `account_ids` (optional): Filter resources by account IDs.
  - `product_names` (recommended): Filter resources by product names (e.g., "Amazon Elastic Compute Cloud", "Amazon Simple Storage Service"). We highly encourage using this filter, or the response will be too heavily polluted.

#### Response Format

The response is a JSON array containing a list of resources in the map migration project. Each resource has the following fields:

- `line_item_resource_id`: The ID or ARN of the resource.
- `region`: The AWS region where the resource is located.
- `account_id`: The AWS account ID associated with the resource.
- `resource_name`: The name of the resource, if it exists.
- `cost`: The cost associated with the resource.

##### Example Response

```json
[
    {
        "line_item_resource_id": "arn:aws:ec2:region:account-id:instance/i-1234567890abcdef0",
        "region": "us-west-2",
        "account_id": "123456789012",
        "resource_name": "My EC2 Instance",
        "cost": 25.50
    },
    {
        "line_item_resource_id": "arn:aws:s3:::my-bucket",
        "region": "us-east-1",
        "account_id": "123456789012",
        "resource_name": "My S3 Bucket",
        "cost": 10.00
    }
]
```

### Usage Examples

- **Example 1**: List All Map Migration Projects
- **Example 2**: Retrieve a Specific Map Migration Project
- **Example 3**: List Products in a Map Migration Project
- **Example 4**: List Resources in a Map Migration Project

--- 

## Solution Provider Endpoints

### 1. List All Map Migration Projects for all your Clients.

- **URL**: `/sp/v3/map-projects/`
- **Method**: `GET`
- **Description**: List All Map Migration Projects for all your Clients.

#### Request Parameters

None.

#### Response Format

The response is a JSON array containing a list of map migration projects. Each project has the following fields:

- `id`: The ID of the MAP Migration project. This ID is nOps specific. nOps assigns a unique ID to each project you onboard to the MAP Dashboard.
- `on_prem_non_commercial_db_entity`: Specific incentives related to on-premises non-commercial database.
- `on_prem_commercial_db_entity`: Specific incentives related to on-premises commercial database.
- `ec2_non_commercial_db_entity`: Specific incentives related to EC2 non-commercial database.
- `ec2_commercial_db_entity`: Specific incentives related to EC2 commercial database.
- `sap_entity`: Specific incentives related to SAP.
- `baseline_s3_entity`: Specific incentives related to S3.
- `speciality_services_entity`: Specific incentives related to speciality services.
- `private_equity_entity`: Specific incentives related to Private Equity.
- `tagged_resources`: Information about tagged resources, including resource count and cost.
- `untagged_resources`: Information about untagged resources, including resource count and cost.
- `credits`: Information about received credits, including records of credit details.
- `client`: The client associated with the MAP project. Name, website and the client ID, which is another nOps specific value that gets generated and assigned to each nOps platform client at the time of account creation in nOps. You will find your assigned Client ID at the top right corner of nOps platform screen under your profile name.
- `created`: The creation date of the project.
- `modified`: The last update date of the project.
- `name`: The name of the MAP project. This is also something you specify while onboarding the MAP project in nOps MAP Dashboard and it can be renamed later as well.
- `migration_id`: The migration ID is identified by the project number in the contract, of the form MPExxxxx (where xxxxx are digits) or a 10-digit identifier such as ABCLMNOPYZ. If you are not sure where to find your MAP Project ID, refer to the top of the first page of your MAP contract.
- `tag_key`: Must be the exact value string from your AWS contract. This value should be mig followed by the 5 or 10-digit MAP ID. If it’s an older MAP contract, you may also use the server-id (typically d- followed by 9 digits).
- `server_id`: The server ID, as referred above.
- `credit_percent`: Prefilled for you to the typical 25%, however you may edit this when creating a project in nOps if your contract terms state a different discount for standard MAP resources.
- `workload_pk`: Primary key of a workload (deprecated).
- `date_start`: The start date of the migration (per your MAP contract terms from AWS).
- `date_end`: The end date of the migration (per your MAP contract terms from AWS).
- `migration_period`: The migration period in months.
- `windows_optimization`: Indicates if Windows optimization is enabled.
- `map_credit_cap`: The MAP credit cap (default is 1.000.000USD).
- 
