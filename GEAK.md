# Review of *GEAK: Introducing Triton Kernel AI Agent & Evaluation Benchmarks*

The paper presents **GEAK (Generating Efficient AI-centric GPU Kernels)**, a modular agent framework developed by AMD to automate the generation of high-performance GPU kernels in the **Triton DSL**, targeting platforms such as **AMD Instinct™ MI300X and MI250 GPUs**. With deep learning workloads growing in scale and complexity, manual kernel optimization is increasingly impractical. GEAK addresses this by combining large language models (LLMs) with **agentic reasoning loops** and **inference-time compute scaling** to produce correct and efficient GPU code.

## Introduction
The authors highlight the gap between LLMs’ general-purpose code generation strengths and their struggles with low-level, performance-critical GPU kernels. They argue that structured agentic frameworks with feedback loops are essential for correctness and efficiency, especially on non-CUDA hardware.

## GEAK Framework
GEAK is built around four cooperating modules:  
- **Generator** – produces Triton code.  
- **Evaluator** – tests correctness and performance.  
- **Reflector** – analyzes error traces and proposes fixes.  
- **Optimizer** – refines correct kernels for latency and efficiency.  

Enhancements include **one-shot prompting with similar code retrieval**, **domain-specific knowledge injection**, **Reflexion-style debugging**, and **parallel/sequential scaling** to maximize accuracy under given compute budgets.

## Benchmarks
Two evaluation suites are introduced:  
1. **TritonBench-revised** – 184 kernels adapted from TritonBench-G with stricter unit tests and AMD GPU compliance.  
2. **ROCm Triton Benchmark** – 30 real-world kernels from open-source AMD repositories with strong test coverage.  

## Experiments
- Direct prompting of GPT-4.1, Gemini 2.5 Pro, and Claude 3.7 yields <15% execution accuracy on TritonBench-revised, often failing on ROCm.  
- **GEAK achieves ~55% execution accuracy and up to 2.59× speedup** on TritonBench-revised, and **63% accuracy on ROCm**.  
- Scaling experiments show consistent improvements with more inference-time iterations and parallel runs.  
- Ablations confirm complementary roles of modules: knowledge injection boosts correctness, one-shot prompting aids accuracy, and the Optimizer significantly improves runtime performance.

## Case Study
A study on the **flip kernel** shows GEAK-generated code reduces memory bandwidth, register pressure, and improves cache efficiency compared to expert-written kernels, achieving a **2.26× speedup**.

## Conclusion
GEAK demonstrates the potential of **agentic, self-improving code generation systems** for GPU kernels. By open-sourcing both the framework and benchmarks, the authors aim to accelerate research, broaden support for non-CUDA hardware, and democratize access to expert-level GPU performance.
