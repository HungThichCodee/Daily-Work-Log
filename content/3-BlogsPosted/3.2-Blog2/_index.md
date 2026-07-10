---
title: "Modernizing ML Pipelines for Agricultural Robots with Aigen"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---
## Modernizing Machine Learning Pipelines for Agricultural Robotics with Aigen and Amazon SageMaker AI

Hello everyone, after researching and synthesizing technical documentation from the AWS Architecture Blog on how Aigen modernizes its machine learning pipeline for agricultural robot fleets, I would like to share this summary blog covering the core points for the community's reference.

## 1. Role and Research Context
Aigen is a company that builds autonomous agricultural robots, using computer vision AI to automatically identify and eliminate herbicide-resistant weeds without chemicals. Powered by renewable energy, they also provide real-time field data to help farmers make better decisions. As the robot fleet scaled, Aigen's on-premise infrastructure became a bottleneck for scaling the model building pipeline. Four main challenges were addressed in this research:
- **Limited Connectivity**: Unstable internet in rural areas makes communication between robots and the cloud difficult.
- **High Data Labeling Costs**: Manually labeling thousands of new data samples every day is expensive and time-consuming.
- **Limited Computing Capacity**: Training specialized edge models and fine-tuning foundation models on on-premise infrastructure (RTX 3090 machines) is restricted by GPU parallelism and power.
- **Scalability Issues**: Model training and batch inference labeling had to compete for the same RTX 3090 cluster, causing delays for both the data science and labeling teams.

## 2. Technical Highlights
Aigen's breakthrough lies in shifting from on-premise infrastructure to an AWS-based cloud-native architecture, with the following technical highlights:

- **4-Tier Model Architecture**: Foundation models (SAM2, Grounding DINO...) → Expert models (Vision Transformer/CNN, tens of millions of parameters) → Student models (FP32, under 1.5 million parameters, optimized using quantization-aware training) → Edge models (1–1.2 million parameters, INT8 quantized, running on a 2.3 TOPS NPU at just ~1.5W).

![Aigen Model Architecture](/images/3-BlogsPosted/3.2-Blog2/fig1.png)
*Figure 1: Aigen Model Architecture*

- **Edge Computing via AWS IoT Core**: Robots use AWS IoT Core to securely push data (video, telemetry, metadata) to Amazon S3 even with poor connectivity.
- **Automated Data Pipeline**: Data is processed via ETL, then automatically labeled using an ensemble of vision foundation models (Grounding DINO, Owl-ViT, SAM2, CLIPSeg) combined with custom-built specialized vision models.
- **Active Learning**: The system selects the most "informative" data samples (where the model is weak or data is diverse) for human review, rather than labeling millions of images every season.
- **Training on Amazon SageMaker AI**: Uses Distributed Data Parallel (DDP) on a multi-GPU cluster, entirely separating training resources from labeling resources, boosting throughput and reducing wait times.

![Aigen modernized architecture](/images/3-BlogsPosted/3.2-Blog2/fig2.png)
*Figure 2: Aigen modernized architecture*

## 3. Business Results Achieved
- **Cost Savings**: Reduced labeling costs from approximately $2.00 to $0.089 per image, representing a 22.5x reduction.
- **Accelerated Labeling Pipeline**: Average labeling time dropped from 14 minutes 57 seconds (manual) to just 41 seconds using SageMaker batch inference, reducing the time to market for new crop models from months to weeks.
- **Accelerated Experimentation**: Experimentation capacity increased from 5 times/week (on-premise) to hundreds of times/week with SageMaker AI, equivalent to a 20x throughput increase.
- **Driving Innovation**: Powerful GPUs on SageMaker AI enable training and fine-tuning advanced Vision Transformer models that previous on-premise infrastructure could not support.

## 4. Lessons Learned and Scalability
- **No Need to Manage GPU Infrastructure**: SageMaker AI removes the need for Aigen to build and maintain auto-scaling GPU infrastructure, allowing them to focus resources on model development.
- **Streamlined ML Lifecycle**: From data preparation to model deployment, leveraging both built-in features and custom workflows like pre-labeling.
- **Flexible Continuous Fine-tuning**: Managed infrastructure allows daily fine-tuning as crops grow through the season, or when expanding to new customers/fields with different soil, lighting, and crop varieties.

## 5. Limitations and Implementation Considerations
- **Human-in-the-loop is Still Required**: The model generates automated pre-labels, but annotators are still needed to review and correct errors; it is not 100% automated yet.
- **Dependence on Foundation Model Quality**: The effectiveness of automated labeling relies heavily on whether foundation models (SAM2, Grounding DINO...) are suitable for the specific agricultural domain.
- **Managing Multiple Seasonal Models**: Each crop type (tomatoes, cotton, sugar beets, soybeans...) requires a specific student model, so managing the lifecycle of numerous specialized models requires strict MLOps processes.
- **Initial Transition Costs**: Despite long-term savings, shifting from on-premise to cloud-native demands upfront investment to redesign the entire pipeline.

## Conclusion
Transitioning from an on-premise pipeline to a cloud-native architecture on AWS, combined with automated labeling via foundation model ensembles and parallel training on Amazon SageMaker AI, has empowered Aigen to overcome connectivity, cost, and compute limitations. This is a quintessential case study demonstrating how generative AI and managed ML services can modernize robotics pipelines for more efficient and sustainable farming.

*Original Authors: Purna Sanyal, Yuri Brigance, and Usman M. Khan (Aigen).*
*Original document: [How Aigen transformed agricultural robotics for sustainable farming with Amazon SageMaker AI](https://aws.amazon.com/blogs/architecture/how-aigen-transformed-agricultural-robotics-for-sustainable-farming-with-amazon-sagemaker-ai/)*