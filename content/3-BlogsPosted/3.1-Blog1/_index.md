---
title: "Optimizing Generative AI Video Inference"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
## Optimizing Generative AI Video Inference: How Synthesia Breakthroughs Performance on Amazon EC2 G7e

Hello everyone, after researching and synthesizing technical documentation from the AWS Architecture Blog on how Synthesia optimizes its AI pipeline, I would like to share this summary blog covering the core points for the community's reference.

## 1. Role and Research Context
Synthesia is an enterprise AI video platform that allows creating synthetic video avatars from real human images and voices without needing cameras or microphones. To operate complex latent diffusion models, Synthesia requires a powerful and flexible hardware infrastructure. This research focuses on solving the bottleneck during video decoding (VAE Decoder), where saving data to disk often stalls the GPU, wasting computational resources.

## 2. Technical Highlights
The most significant breakthrough in this research is the application of the Asynchronous Frame Generation Pipeline technique, replacing the traditional sequential process. 

Key technical features include:
- **Using Amazon EC2 G7e instances**: Equipped with NVIDIA RTX PRO 6000 Blackwell GPUs with 96GB of GPU memory, optimized for memory-intensive Generative AI models.
- **Dual CUDA Streams Mechanism**: Separates the Compute Stream and Copy Stream, allowing the GPU to process new frames while simultaneously transferring old frame data to the CPU.
- **Double-Buffer Strategy and Dedicated Processing Thread**: Utilizes two memory buffers and a dedicated CPU Worker thread to write data to disk, preventing the main Python thread from blocking.
- **Outstanding Efficiency**: Increased GPU core utilization from 82% to 99.9%, reduced latency by 8.2%, and increased video processing throughput.

## 3. Practical Applications
This solution is not just theoretical but brings specific economic and operational value:
- **Cost Savings**: Based on calculations on the g7e.2xlarge instance, this technique saves about $896 for every 1,000 hours of decoded video.
- **Pipeline Optimization**: Effectively applied to modern video models like Wan 2.2 14B without needing to change model weights or sacrificing image quality.
- **Handling Long Videos**: Thanks to chunk-based decoding, the system can process videos of any length without being limited by GPU memory capacity.

## 4. Scalability
The research confirms that this technique has extremely high flexibility:
- **Architecture Independent**: This method is not exclusive to the Wan model but can be applied to any chunk-based video generation pipeline that transfers frames to host memory.
- **Hardware Independent**: Although tested on G7e, this technique can be extended to different types of GPUs.
- **Future Potential**: Performance could be even more impressive when combined with compiled models to fully leverage hardware power.

## 5. Limitations and Implementation Considerations
- **Theoretical Savings**: The $896/1,000 hours figure is an ideal optimum, assuming the system encounters no other bottlenecks besides decoding.
- **Technical Complexity**: Requires strict synchronization mechanisms using CUDA Events to avoid data corruption when multiple streams access memory simultaneously.
- **Optimization Scope**: Only solves the issue at the VAE decoder and data transfer, it does not accelerate the diffusion phase of the model.
- **GPU Memory**: Resource occupation remains proportional to the chunk size you choose to process.

## 6. Deep Dive Guide

### Understanding the Sequential Decoding Bottleneck
Latent diffusion video generation models perform the diffusion process in a compressed latent space of a variational auto-encoder (VAE) to reduce compute and memory requirements.

![Fig 1: High-level architecture of a VAE model](/images/3-BlogsPosted/3.1-Blog1/fig1.png)
*Fig 1: High-level architecture of a VAE model.*

After the final denoising step, the generated video is still represented in the latent space. The last step consists in decoding this latent video back into a human-readable pixel video. The video is usually split along the temporal dimension and decoded one latent frame at a time, resulting in a chunk of, for example, 4 consecutive pixel frames.

![Fig 2: Decoding one latent](/images/3-BlogsPosted/3.1-Blog1/fig2.png)
*Fig 2: Decoding one latent into 4 pixel frames and transferring to host.*

Traditionally, a newly generated set of frames in a chunk (Chunk N) is passed from GPU memory to CPU synchronously, and it must be committed to storage before the CPU can launch the CUDA kernels that process Chunk N+1. This leads to systematic GPU stalls.

![Fig 3: Sequential Frame Generation Pipeline](/images/3-BlogsPosted/3.1-Blog1/fig3.png)
*Fig 3: Sequential Pipeline causing GPU stalls.*

### Asynchronous Frame Generation Pipeline
To minimize GPU stalling, the pipeline is modified so that all host-side CPU work (such as writing to a file) runs in parallel with a stream of uninterrupted device-side kernels. This is achieved using two CUDA streams: Compute Stream and Copy Stream.

![Fig 4: High level diagram](/images/3-BlogsPosted/3.1-Blog1/fig4.png)
*Fig 4: Key components in the Asynchronous Frame Generation Pipeline.*

To avoid host-side blocking calls and maximize GPU utilization, two mechanisms are introduced:
1. **Dedicated Worker CPU thread**: Responsible for reading chunks from RAM and writing them to file, leaving the main Python thread to focus on launching kernels and scheduling device-to-host (D2H) transfers.
2. **Double-Buffer strategy**: Utilizes two memory buffers on the GPU Memory (VRAM) and on the Host Memory (Pinned RAM) to make sure compute, D2H transfers, and host processing overlap safely.

![Fig 5: Schematic representation of Events, Streams, Buffers](/images/3-BlogsPosted/3.1-Blog1/fig5.png)
*Fig 5: Interplay between Events, Streams, Buffers, and Worker (Events prevent data corruption).*

Thanks to this pipeline, the GPU kernel utilization increased from 82% to 99.9%, completely eliminating wasted idle time during video decoding. This is clearly shown in the profiling graphs below:

![Fig 6: Synchronous pipeline profiling](/images/3-BlogsPosted/3.1-Blog1/fig6.png)
*Fig 6: Profile of the Synchronous pipeline. The GPU stream stalls waiting for the CPU to write Chunk N to disk.*

![Fig 7: Asynchronous pipeline profiling](/images/3-BlogsPosted/3.1-Blog1/fig7.png)
*Fig 7: In the Asynchronous pipeline, the Compute Stream is not interrupted by the copying and writing of the frames.*

The table below shows the benchmark results for decoding time over 10 consecutive runs:

| Metric   | Synchronous (time s / video) | Asynchronous (time s / video) |
| -------- | ---------------------------- | ----------------------------- |
| **min**  | 21.98                        | 20.16                         |
| **mean** | 21.99                        | 20.17                         |
| **P99**  | 22.01                        | 20.20                         |

*Table 1: Benchmark results for 10 consecutive decoding runs for the Synchronous and Asynchronous pipelines.*

## Conclusion
The transition from sequential to asynchronous processing is an important step forward in operating Generative AI. By fully leveraging the parallel capabilities of GPUs on Amazon EC2 G7e, we not only accelerate video production but also significantly optimize operational costs. This is a valuable lesson for anyone building large-scale AI applications.

Hopefully, this summary gives you a quick and deep insight into how "tech giants" like Synthesia are optimizing their infrastructure!

*Original Authors: Moises Hernandez, Hanno Bever, Emanuele Levi, and Pierre Lienhart.*
*Original document: [How Synthesia optimizes Generative AI video inference on Amazon EC2 G7e instances](https://aws.amazon.com/blogs/architecture/how-synthesia-optimizes-generative-ai-video-inference-on-amazon-ec2-g7e-instances/)*