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

1. **Navigate to CloudFormation service**
   - Go to your Container Cost CloudFormation stack (e.g. `container-cost-setup-<your_Account_ID>`), and ensure you're in the correct AWS region where the stack was initially created.
   
   ![CloudFormation Stack Navigation](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.48.50%E2%80%AFp.m..png)

2. **Update CloudFormation Stack**
   - Click the **Update** button in the upper right corner.
   
   ![Update Button](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.49.24%E2%80%AFp.m..png)

   - Select **Replace existing template**.

   ![Replace Existing Template](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.49.49%E2%80%AFp.m..png)

   - Choose **Amazon S3 URL** and use the following template URL: [template](https://nops-rules-lambda-sources.s3.us-west-2.amazonaws.com/container_cost/container-cost-setup-migration.yaml).

   ![S3 URL Template](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.50.09%E2%80%AFp.m..png)

   - Click **Next** and keep the parameters unchanged. Continue by clicking **Next** again.

   ![Parameters](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.50.33%E2%80%AFp.m..png)

   - Scroll down and tick the box **"I acknowledge that AWS CloudFormation might create IAM resources with custom names."** Then click **Submit**.

   ![Acknowledge IAM Resources](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.51.35%E2%80%AFp.m..png)

   - Wait for the update to complete successfully.

3. **Invoke Lambda to Update Trust Relationship Policy (Optional)**
   - To update the IAM role trust relationship immediately, you can manually invoke the management IAM role Lambda. By default, it runs every 2 hours to create and/or update the necessary IAM roles and policies for the Container Insights agent.
   - Go to the Lambda functions and search for: `nops-container-cost-agent-role-management`. Ensure you are in the same AWS region as the CloudFormation stack.

   ![Lambda Search](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-12+at+10.26.08%E2%80%AFp.m..png)

   - Click on the **Test** tab, then click the **Test** button to invoke the Lambda with a test event JSON payload.

   ![Invoke Lambda](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-12+at+10.25.52%E2%80%AFp.m..png)

   - You should see log messages indicating the IAM role trust relationship has been updated.

4. **Download Cleanup Script**
   - Download the cleanup script from [here](https://nops-help-site-assets.s3.amazonaws.com/scripts/nops-cleanup.sh).

5. **Execute Cleanup Script**
   - Grant execution permissions to the script.
   ```bash
   chmod +x nops-cleanup.sh
    ```
    - Open your Unix-like terminal.
    - Change/Switch context to your desired cluster.
    - Replace positional parameters with your own and run the script.
    ```bash
    1. EKS Cluster ARN
    2. S3 Bucket name
    3. Install karpenops agent (true if you are already using our Karpenter agent and want to migrate it or false to not install it)
    4. API Key (Get it from the nOps platform in your Cluster Configuration page, required only if karpenops agent install is true)
    5. ClusterID (Get it from the nOps platform in your Cluster Configuration page, required only if karpenops agent install is true)
    ```
    Example:
    ```bash
    ./nops-cleanup.sh arn:aws:eks:us-east-1:123456789101:cluster/example-cluster nops-container-cost-123456789101 true 1234.a1234a1a123ab1a01234a12a1a1ab1ab a+ABC1
    ```
    - Wait for the script to successfully finish.
***Note: If the script gets stuck when deleting the namespaces, you can refer to [Unable to destroy namespaces](#unable-to-destroy-namespaces).***

    6. **Verify new agent**
        - Confirm new namespace and resources.
        ```bash
        kubectl -n nops get pods
        ```

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