---
title: Migration nOps Kubernetes Agent for Container Insights
keywords: CostContainer, integrations, agent, helm, containers, insights
tags: [agents_integrations]
sidebar: mydoc_sidebar
permalink: Migration-nOps-Kubernetes-Agent-on-EKS.html
folder: Integrations_and_Agents
series: [agents_integrations]
weight: 6.0
---
If you come from a previous installation of the Container Insights Kubernetes agent (Using a bash script), new installation method requires an update in the CloudFormation Stack which creates a Lambda that takes care of the Creation/Update of the IRSA used by the agent to push metrics into the S3 Bucket dedicated for your AWS account, and running a cleanup script to remove the old helm release and deleting the unused namespaces.
1. TOC
{:toc}
# Prerequisites
1. Make sure you have access to the Kubernetes cluster (recommended version v1.23.6 or later) to deploy the agent.
2. Have a previous version of the agent installed in your cluster, you can verify this by listing the namespaces and looking for the following: **nops-prometheus-system**, **nops-cost** and **nops-k8s-agent**.
3. <a href="https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html" target="_blank">aws cli</a>
4. <a href="https://helm.sh/" target="_blank">Helm</a>
5. <a href="https://kubernetes.io/docs/reference/kubectl/overview/" target="_blank">kubectl</a>
6. Unix-like terminal to execute the installation script.
# Update CloudFormation Stack
1. **Navigate to CloudFormation service**
    - Go to your container cost cloudformation stack (e.g. container-cost-setup-<your_Account_ID>, remember to change your AWS region where you initially create the stack
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.48.50%E2%80%AFp.m..png)
2. **Update CloudFormation Stack**
    - Click the upper right **Update** button
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.49.24%E2%80%AFp.m..png)
    - Select the **Replace existing template**
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.49.49%E2%80%AFp.m..png)
    - Select Amazon S3 URL and for the URL use the following: https://nops-rules-lambda-sources.s3.us-west-2.amazonaws.com/container_cost/container-cost-setup-migration.yaml
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.50.09%E2%80%AFp.m..png)
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.50.21%E2%80%AFp.m..png)
    - Click on Next
    - Keep the parameters as they are and click Next
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.50.33%E2%80%AFp.m..png)
    - Scroll down and click Next
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.50.49%E2%80%AFp.m..png)
    - Scroll down and you will see changes in the set preview section, tick the box ***"I acknowledge that AWS CloudFormation might create IAM resources with custom names."*** and click on Submit
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.51.35%E2%80%AFp.m..png)
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-05+at+1.51.24%E2%80%AFp.m..png)
    - Wait for the update to be successfully completed
3. **Invoke Lambda to Update Trust Relationship Policy (OPTIONAL)**
    - If you want to update the IAM role trust relationship inmediately you can invoke the management IAM role lambda (Normal Schedule is to run every 2 hours to create and/or update the neccesary IAM roles and IAM policies for container insights agent).
    - Go to Lambda functions and search for: nops-container-cost-agent-role-management (The region needs to be the same as the CloudFormation stack was deployed to)
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-12+at+10.26.08%E2%80%AFp.m..png)
    - Click on the Test tab, and click on the Test Button to invoke the lambda with a test event JSON payload.
    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-insights-migration/Screenshot+2024-08-12+at+10.25.52%E2%80%AFp.m..png)
    - Lambda should be invoked and in a few seconds you should see the IAM role trust relantionship updated log messages from the Lambda execution.
4. **Download clean up script**
    - Download clean up script from here: https://nops-help-site-assets.s3.amazonaws.com/scripts/nops-cleanup.sh
5. **Execute clean up script**
    - Give execution permission to the script.
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
    ***Note: If the script get stuck when deleting the namespaces you can refer to [this]().***
6. **Verify new agent**
    - Confirm new namespace and resources.
      ```bash
      kubectl -n nops get pods
      ```

### Unable to destroy namespaces
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