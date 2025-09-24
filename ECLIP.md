# Review of *ECLIP: Energy-efficient and Practical Co-Location of ML Inference on Spatially Partitioned GPUs*

The paper introduces **ECLIP**, a framework designed to improve the **energy efficiency and throughput of ML inference servers** by enabling practical kernel-level spatial partitioning on GPUs. Traditional inference servers often underutilize GPU resources, wasting power through idle compute units, while existing partitioning techniques either rely on coarse-grain model-level allocation or require impractical hardware modifications.

## Introduction
The authors highlight the problem: GPUs are critical for ML inference but are often inefficiently utilized. Spatial partitioning can mitigate contention when models share a GPU, yet current solutions face significant overheads or impracticalities. **ECLIP** is proposed as a practical alternative, leveraging AMD’s CU masking capabilities.

## ECLIP Framework
The framework combines three key innovations:
1. **Pre-allocated CU-masked streams** to avoid costly per-kernel partitioning overheads.  
2. A **runtime scheduler** that redirects kernels to appropriate CU partitions while preserving data dependencies via injected barrier packets.  
3. A **resource allocation optimizer** that groups kernels, assigns energy-aware CU configurations, and limits repartitioning events to reduce synchronization overhead.  

This design strikes a balance between fine-grained kernel-level partitioning and manageable overheads.

## Experiments
Experiments are conducted on an AMD MI50 GPU with seven common inference models (e.g., Albert, ResNet, VGG). Workloads test different mixes of co-located models under four spatial partitioning scenarios:  
- **Baseline**: No partitioning.  
- **Model-wise**: Coarse-grain partitioning per model.  
- **Kernel-wise IOCTL**: Per-kernel repartitioning with high overhead.  
- **Kernel-wise Preallocation**: Pre-allocated streams, but without optimization.  
- **ECLIP**: Full implementation with optimized partitioning.  

### Results
- **Throughput** improves by up to **21%**, averaging **13%** across workloads.  
- **Energy efficiency** improves by up to **35%**, averaging **25%**.  
- **Tail latency** remains controlled by limiting CU switches, unlike naïve kernel-wise approaches.  

## Related Work
The authors position ECLIP against prior work such as GPUlet, GSlice, KRISP, and ElasticRoom. While these explored kernel-grain partitioning, most required hardware extensions or intrusive framework modifications. **ECLIP achieves similar benefits without architectural changes**, making it deployable in real systems.

## Conclusion
ECLIP provides a **practical and deployable solution** for co-located ML inference, demonstrating consistent performance and energy gains while maintaining acceptable latency. The work represents a step forward in bridging the gap between theoretical partitioning strategies and their applicability on real GPU hardware.
