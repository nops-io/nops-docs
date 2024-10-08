---
title: Migrating nOps Kubernetes Agent for Container Insights
keywords: CostContainer, integrations, agent, helm, containers, insights
tags: [agents_integrations]
sidebar: mydoc_sidebar
permalink: Migration-nOps-Kubernetes-Agent-on-EKS.html
folder: Integrations_and_Agents
series: [agents_integrations]
weight: 6.0
---

If you're migrating from a previous installation of the Container Insights Kubernetes agent (originally installed using a bash script), the new installation method involves updating the CloudFormation Stack. This update creates a Lambda function that manages the creation and update of the IAM Roles for Service Accounts (IRSA) required by the agent to push metrics to the S3 Bucket associated with your AWS account. Additionally, you'll need to run a cleanup script to remove the old Helm release and delete any unused namespaces from the previous installation.

{:toc}

## Prerequisites 

1. Access to the Kubernetes cluster (recommended version v1.23.6 or later) to deploy the agent.
2. A previous version of the agent installed in your cluster. You can verify this by listing the namespaces and looking for the following: **nops-prometheus-system**, **nops-cost**, and **nops-k8s-agent**.
3. [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
4. [Helm](https://helm.sh/)
5. [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/)
6. Unix-like terminal to execute the installation script.

## Updating CloudFormation Stack

   ![Update gif](https://nops-help-site-assets.s3.amazonaws.com/images/upgrade-stack-container-cost.gif)


1. **Navigate to CloudFormation service**
   - Go to your Container Cost CloudFormation stack (e.g. `container-cost-setup-<your_Account_ID>`), and ensure you're in the correct AWS region where the stack was initially created.
   

2. **Update CloudFormation Stack**
   - Click the **Update** button in the upper right corner.
   
   - Select **Replace existing template**.

   - Choose **Amazon S3 URL** and paste the following template URL: **https://nops-rules-lambda-sources.s3.us-west-2.amazonaws.com/container_cost/container-cost-setup-migration.yaml**

   - Click **Next** and keep the parameters unchanged. Continue by clicking **Next** again.

   - Scroll down and tick the box **"I acknowledge that AWS CloudFormation might create IAM resources with custom names."** Then click **Submit**.

   - Wait for the update to complete successfully.


## Clean up and installation script #

1. **Save Script**
   - Save the script from [here](https://nops-help-site-assets.s3.amazonaws.com/scripts/nops-cleanup.sh)

2. **Execute Script**
   - Grant execution permissions to the script.
   ```bash
   chmod +x nops-cleanup.sh
    ```
    - Open your Unix-like terminal.
    - Change/Switch context to your desired cluster.
    - Replace parameters with your own and run the script.
    ```bash

| Parameter               | Description                                                                                     |
|-------------------------|-------------------------------------------------------------------------------------------------|
| `--datadog-api-key`      | Datadog API Key                                                                                 |
| `--container-insights`   | Enabling or not (true or false) container insights                                              |
| `--eks-cluster-arn`      | Target EKS Cluster ARN                                                                          |
| `--s3-bucket-name`       | S3 Bucket name                                                                                  |
| `--karpenops-enabled`    | Install KarpenOps agent (true or false)                                                         |
| `--api-key`              | API Key (Get it from the nOps platform onboarding process, required only if KarpenOps agent install is true). |
| `--cluster-id`           | ClusterID (Get it from the nOps platform onboarding process, required only if KarpenOps agent install is true) |


   Example:

   ```bash
   ./nops-cleanup.sh  --datadog-api-key=a1234ab12a12abc1a123ab123a12a12a --container-insights=true --eks-cluster-arn=arn:aws:eks:us-east-1:123456789101:cluster/example-cluster --s3-bucket-name=nops-container-cost-123456789101 --karpenops-enabled=true --api-key=1234.a1234a1a123ab1a01234a12a1a1ab1ab --cluster-id=a+ABC1
   ```

   Wait for the script to successfully finish.

3. **Verify new agent(s)**
    - Confirm new namespace and resources.
    ```bash
    kubectl -n nops get pods
    ```

    {% include note.html content="If the script gets stuck when deleting the namespaces, you can refer to [Unable to destroy namespaces](#unable-to-destroy-namespaces)." %}

## Unable to destroy namespaces #
In some cases, when trying to delete the namespaces (nops-prometheus-system, nops-cost, nops-k8s-agent) they may end up stuck with a "Terminating" status, for such cases you can do the following:
1. Open a new terminal
2. Change/Switch context to your desired cluster.
3. Run:
   ```bash
   kubectl proxy
   ```
4. Open a second terminal
5. Change/Switch context to your desired cluster.
6. Run:
   ```bash
   kubectl get ns nops-prometheus-system -o json | \
   jq '.spec.finalizers=[]' | \
   curl -X PUT http://localhost:8001/api/v1/namespaces/nops-prometheus-system/finalize -H "Content-Type: application/json" --data @-
   ```
   ```bash
   kubectl get ns nops-cost -o json | \
   jq '.spec.finalizers=[]' | \
   curl -X PUT http://localhost:8001/api/v1/namespaces/nops-cost/finalize -H "Content-Type: application/json" --data @-
   ```
   ```bash
   kubectl get ns nops-k8s-agent -o json | \
   jq '.spec.finalizers=[]' | \
   curl -X PUT http://localhost:8001/api/v1/namespaces/nops-k8s-agent/finalize -H "Content-Type: application/json" --data @-
   ```