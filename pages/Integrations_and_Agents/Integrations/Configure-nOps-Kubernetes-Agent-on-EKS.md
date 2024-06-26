---
title: Configure nOps Kubernetes Agent for Container Cost
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

5. **Copy Generated Script, Configure, and Run It**

    1. Replace the cluster ARN in the script:
        ```sh
        APP_NOPS_K8S_AGENT_CLUSTER_ARN="<REPLACE-YourClusterARN>" # You can find this on your EKS dashboard on AWS
        ```

    2. If you chose to use an IAM User, perform the following extra steps:
        ```sh
        USE_SECRETS=false # Set this to true if you don't want to use identity provider service role
        AGENT_AWS_ACCESS_KEY_ID="<REPLACE-YourAccessKeyId>"
        AGENT_AWS_SECRET_ACCESS_KEY="<REPLACE-YourSecretAccessKey>"
        ```

    3. Make the script executable:
        ```sh
        chmod +x script_name.sh
        ```

    4. Run the script:
        ```sh
        ./install-nops-k8s-agent.sh
        ```
        - If you need IPv6, add the `--ipv6` parameter:
            ```sh
            ./install-nops-k8s-agent.sh --ipv6
            ```
        - If you need to use a custom registry (can't use public repositories directly), use the `--custom-registry` parameter. This will prompt you for the custom registry URL during the installation process.

---

After successful configuration, you'll gain clear visibility into your workloads. While EKS costs are often unclear, nOps helps by automatically finding waste through CPU and memory metrics. This lets you optimize resources and save money quickly.


# Frequently Asked Questions

1. **Will the agent installation affect my existing Prometheus setup?**

    **Answer:** No, the agent installs its own Prometheus instance in a separate namespace, ensuring that it does not interfere with your current Prometheus deployments.

2. **Can I use a custom registry to download container images?**

    **Answer:** Yes, you need to use the `--custom-registry` parameter on the installation script execution.


