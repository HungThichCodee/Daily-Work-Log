---
title: "GPU Cost Attribution in Amazon EKS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
## GPU Cost Attribution in Amazon EKS for FinOps Optimization

Hello AWS Study Group VN! While researching infrastructure cost optimization (FinOps) for AI/ML systems, I read quite an interesting article from the AWS Blog about a solution for a common challenge: **GPU Cost Attribution in Amazon EKS**. 

What I found remarkable is that as enterprises race to provision infrastructure for AI, GPU costs often skyrocket, yet managing and breaking down the "bill" for each team remains incredibly ambiguous because GPU resources are harder to partition than traditional CPU/RAM.

## 1. The Challenge of GPU Cost Management
When deploying Machine Learning or Inference models on AWS, we often build large Amazon EKS clusters and share them across multiple departments to optimize resources. For example, in an enterprise:
- **Team A (Research)**: Runs experiments for new models.
- **Team B (Production AI)**: Runs inference serving real users.
- **Team C (Data Science)**: Processes massive datasets.

All these workloads share a common pool of GPU nodes (such as P4 or G5 instances). The headache for Operations/FinOps teams is: *How do we know exactly which Pod, in which Namespace, belonging to which Team, is consuming how much GPU money?*

Without a clear cost attribution mechanism, businesses face:
- **Hidden Costs**: Bills must be split evenly or estimated manually, failing to reflect actual usage.
- **Resource Waste (Over-provisioning)**: Teams tend to over-request resources "just to be safe", leading to GPUs running idle while costs are still incurred.
- **Optimization Difficulty**: It's impossible to pinpoint exactly where resources are being wasted to make targeted cuts.

## 2. The Proposed AWS Solution
The brilliance of the article is that AWS provides a model to separate GPU costs into 3 highly transparent layers based on open-source Observability tools:
- **Allocated Cost**: Calculated based on the resources requested by the Pod (Kubernetes resource requests).
- **Effective Cost**: Calculated based on the actual GPU utilization of that Pod.
- **Waste Cost**: The difference between what they requested and what they actually used. This is the "golden" data for DevOps to optimize configurations.

Technically, this architecture leverages NVIDIA's Multi-Instance GPU (MIG) feature to partition physical GPUs, then uses metric collection tools from the hardware level up to the Kubernetes Pod level:

![Architecture flow](/images/3-BlogsPosted/3.3-Blog3/fig1.png)
*Figure 1: Architecture flow*

- **NVIDIA DCGM Exporter**: Collects GPU hardware metrics (performance, memory).
- **Kube-State-Metrics**: Provides Kubernetes context (which Pod belongs to which Namespace/Team via Labels).
- **OpenTelemetry Collector**: Acts as a standardized pipeline, merging the two metric sources and pushing them to Prometheus.
- **Amazon Managed Grafana**: Visualizes all data onto Dashboards for real-time tracking of costs by Business Unit (BU).

![Grafana Dashboard](/images/3-BlogsPosted/3.3-Blog3/fig2.png)
*Figure 2: Grafana dashboard showing the cost attribution*

## 3. AWS Services Featured in the Architecture
- **Amazon Elastic Kubernetes Service (Amazon EKS)**
- **AWS Distro for OpenTelemetry (ADOT)**
- **Amazon Managed Service for Prometheus**
- **Amazon Managed Grafana**
- **AWS IAM** (using IRSA for secure permission assignment for metric collection services)

## 4. Key Learnings
What I found most interesting about this article is that cost management in the AI/ML era isn't just about turning on AWS Budgets to view the total bill at the end of the month.
- **FinOps must be tied to Observability**: To optimize costs accurately, you must have granular technical monitoring metrics down to individual Pods.
- **Leverage Managed Open Source**: AWS providing Managed versions of Prometheus and Grafana significantly reduces the operational burden of maintaining metric data pipelines for DevOps teams, allowing them to focus solely on dashboard analysis.
- **Shift the Provisioning Mindset**: Looking at the "Waste Cost" chart, operations teams have compelling data to request Data Science teams to re-optimize resource allocation limits, avoiding wasting "big bucks" on expensive GPU nodes.

## Conclusion
This architectural solution provides an excellent tool for clearly identifying and attributing GPU costs to individual user groups on Amazon EKS, providing granular visibility and driving practical FinOps optimization.

*Original Author: Siva Guruvareddiar.*
*Original document: [GPU cost attribution in Amazon EKS using Amazon Managed Service for Prometheus, Amazon Managed Grafana, and OpenTelemetry](https://aws.amazon.com/blogs/mt/gpu-cost-attribution-in-amazon-eks-using-amazon-managed-service-for-prometheus-amazon-managed-grafana-and-opentelemetry/)*