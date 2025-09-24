# Review of *Ansor: Generating High-Performance Tensor Programs for Deep Learning*

The paper introduces **Ansor**, a framework for automatically generating high-performance tensor programs, a critical component for efficient execution of deep neural networks (DNNs). Unlike traditional approaches that rely on **vendor-provided libraries** (e.g., cuDNN, MKL-DNN) or **template-guided search strategies**, Ansor explores a much larger search space with an automated hierarchical representation, enabling it to discover optimization combinations beyond existing methods.

In the **introduction**, the authors highlight the difficulty of obtaining performant tensor programs across diverse operators and hardware platforms. Manual templates require extensive engineering and expertise, while existing auto-schedulers often prune too aggressively or rely on incomplete program estimates, missing effective optimizations.

The **Ansor framework** is built around three key components:  
- A **program sampler**, which uses a **hierarchical search space**. It defines high-level structures as *sketches* and leaves billions of low-level details (e.g., tile size, unroll factors) as annotations. Programs are sampled randomly to ensure broad coverage.  
- A **performance tuner**, which fine-tunes sampled programs using **evolutionary search** and a **learned cost model**. This avoids relying on inaccurate estimates of incomplete programs.  
- A **task scheduler**, which applies a gradient-descent–based strategy to allocate search time to the most critical subgraphs, improving end-to-end performance of DNNs.

The paper’s **experiments** span three levels:  

1. **Single Operators:** Ansor outperforms state-of-the-art systems (AutoTVM, FlexTensor, Halide auto-scheduler) across a variety of workloads, achieving speedups up to 22× in certain operators.  
2. **Subgraphs:** On common deep learning subgraphs such as convolution + batchnorm + ReLU, and attention-like structures, Ansor shows 1.1–14× improvements relative to baselines on both CPUs and GPUs.  
3. **End-to-End Networks:** On full models including ResNet-50, MobileNet-V2, BERT, and DCGAN, Ansor improves inference speed by up to **3.8× on Intel CPUs**, **2.6× on ARM CPUs**, and **1.7× on NVIDIA GPUs**, consistently outperforming both manual libraries and AutoTVM.  

The authors also conduct **ablation studies**, showing that both the **expanded search space** and **fine-tuning strategy** are essential. Without either, performance drops significantly. The task scheduler further improves efficiency by prioritizing bottleneck subgraphs, yielding faster convergence.

In **conclusion**, Ansor demonstrates that it is possible to achieve state-of-the-art performance across diverse hardware without manual templates or restrictive search strategies. Its combination of a hierarchical search space, evolutionary fine-tuning, and task scheduling establishes a general-purpose framework for optimizing tensor programs, reducing engineering effort and enabling broader deployment of efficient DNNs.
