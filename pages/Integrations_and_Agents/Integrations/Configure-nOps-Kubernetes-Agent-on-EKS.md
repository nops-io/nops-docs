---
title: Configure nOps Kubernetes Agent for Container Insights
keywords: CostContainer, integrations
tags: [agents_integrations]
sidebar: mydoc_sidebar
permalink: Configure-nOps-Kubernetes-Agent-on-EKS.html
folder: Integrations_and_Agents
series: [agents_integrations]
weight: 6.0
---

Installing the Container Cost Kubernetes agent is required to get access to container level visibility.

The way it works is by collecting metrics from the cluster and storing them into a S3 bucket on your AWS account which then is copied
over to nOps.

1. TOC
{:toc}

# Prerequisites

1. You must have an active nOps account. If you do not have one, please register on <a href="https://app.nops.io/" target="_blank">nOps</a>
2. Make sure you have access to the Kubernetes cluster (recommended version v1.23.6 or later) to deploy the agent.
3. <a href="https://helm.sh/" target="_blank">Helm</a>
4. <a href="https://kubernetes.io/docs/reference/kubectl/overview/" target="_blank">kubectl</a>
5. Unix-like terminal to execute the installation script.


# Steps to Configure nOps Kubernetes Agent

1. **Navigate to Container Cost Tab**
    - Go to your [Container Cost Integrations Settings](https://app.nops.io/v3/settings?tab=Integrations&subTab=Container-Cost).
    - ![Organization Settings](https://nops-help-site-assets.s3.amazonaws.com/images/integrations-container-cost.png)

2. **Setup Container Cost Integration**
    - Click the **Setup** button for the desired account. Ensure you are authenticated into that account.
    - This step will:
        - Create an S3 bucket in your AWS account.
        - Grant writing permissions to the agent for writing files to that bucket.
        - Use either your OIDC Identity Provider to create a service role or an IAM User with permissions to write to the S3 bucket.
        - Allow nOps to copy those files.

3. **Configure CloudFormation Stack**
    - On the CloudFormation stack creation page:
        - List the regions where your clusters for that specific account are located, separated by commas (e.g., `us-east-1,us-east-2,us-west-1,us-west-2`).
        - *(Optional)* If you don't have an IAM OIDC provider configured, you can create an IAM User to grant the agent permissions to write to the S3 bucket. To do this, set `CreateIAMUser` to `true`.
            If you choose this approach, you must create and store secret key credentials to replace values in the installation script.
        - Select the **I acknowledge that AWS CloudFormation might create IAM resources with custom names** checkbox.
        - Click the **Create stack** button.
    - ![CloudFormation Stack Parameters](https://nops-help-site-assets.s3.amazonaws.com/images/integration-container-cost-cf-parameters.png)

4. **Check Integration Status**
    - After the creation is complete, return to the nOps platform.
    - Click the **Check Status** button to verify the integration status.


    ![](https://nops-help-site-assets.s3.amazonaws.com/images/container-cost-integration-check-status.gif)

### 5. **Install Agent**

1. **Click on the Install Agent Button**
    - In the nOps platform, click on the **Install Agent** button.
    - A script will be generated for you to execute.

2. **Select Options**
    - Before copying the script, you have the option to enable or disable:
        - Debug mode
        - IPv6 support
    - Check or uncheck these options based on your requirements.

3. **Copy the Installation Script**
    - Click the **Copy** button to copy the generated installation script.

4. **Modify the Installation Script**
    - Open a text editor and paste the copied script.
    - Replace the placeholder `<<REPLACE-CLUSTER-ARN>>` with your actual cluster ARN in the following line:
        ```sh
        --set nopsAgent.env_variables.APP_NOPS_K8S_AGENT_CLUSTER_ARN=<<REPLACE-CLUSTER-ARN>>
        ```

5. **Add IAM User Credentials (If Applicable)**
    - If you chose to use an IAM User instead of an IAM Role, you need to add the following lines to the script with your access key ID and secret access key:
        ```sh
        --set nopsAgent.secrets.useAwsCredentials=true \
        --set nopsAgent.secrets.awsAccessKeyId=<<REPLACE-YOUR-ACCESS-KEY-ID>> \
        --set nopsAgent.secrets.awsSecretAccessKey=<<REPLACE-YOUR-SECRET-ACCESS-KEY>>
        ```

6. **Execute the Installation Script**
    - Open your Unix-like terminal.
    - Execute the modified command by pasting it into the terminal and pressing **Enter**.

After running the script, the nOps Kubernetes agent will be installed on your cluster, allowing you to gain detailed visibility into your container costs.


---

After successful configuration, you'll gain clear visibility into your workloads. While EKS costs are often unclear, nOps helps by automatically finding waste through CPU and memory metrics. This lets you optimize resources and save money quickly.

# Removing Agent

To remove the agent from your cluster you just need to follow these steps:


```bash
# Delete nops-k8s-agent release
helm uninstall nops-k8s-agent --namespace nops-k8s-agent

# Delete nops-cost release
helm uninstall nops-cost --namespace nops-cost

# Delete nops-prometheus release
helm uninstall nops-prometheus --namespace nops-prometheus-system

# Delete nops-prometheus-crds release
helm uninstall nops-prometheus-crds --namespace nops-prometheus-system

# Delete nops-gpu-metrics-exporter release
helm uninstall nops-gpu-metrics-exporter --namespace nops-prometheus-system

# Delete the namespaces
kubectl delete namespace nops-k8s-agent
kubectl delete namespace nops-cost
kubectl delete namespace nops-prometheus-system
```
    




# Frequently Asked Questions

1. **Will the agent installation affect my existing Prometheus setup?**

    **Answer:** No, the agent installs its own Prometheus instance in a separate namespace, ensuring that it does not interfere with your current Prometheus deployments.

2. **Can I use a custom registry to download container images?**

    **Answer:** Yes, you need to use the `--custom-registry` parameter on the installation script execution.


