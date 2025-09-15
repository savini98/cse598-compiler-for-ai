# Class Review Summary: *Relay: A High-Level Compiler for Deep Learning*

The paper introduces **Relay**, a new compiler framework for deep learning that provides a **functional, statically typed intermediate representation (IR)**. Unlike many existing deep learning IRs, which are specialized for particular models or hardware, Relay is designed to unify and generalize them, making it easier to express advanced models, compose optimizations, and target diverse hardware backends.

Relay addresses three key challenges in deep learning compilers:  

1. **Expressivity** – Relay can represent complex models using features like control flow, recursion, higher-order functions, and algebraic data types (trees, lists, graphs). This is much richer than traditional computation graph IRs.  
2. **Composability** – Relay treats common framework features (quantization, shape inference, operator fusion) as compiler passes, so they can be combined systematically instead of relying on ad hoc extensions.  
3. **Portability** – By separating operators from their implementations, Relay can target CPUs, GPUs, and emerging accelerators like FPGAs, generating optimized code across platforms.  

## Design and Optimizations
Relay builds on principles from functional programming languages like OCaml and SML. Its IR includes tensors as first-class citizens and a type system that tracks tensor shapes, enabling strong correctness guarantees and optimization opportunities. The compiler pipeline includes:  
- **Frontend importers** (from frameworks like TensorFlow, PyTorch, and ONNX),  
- **Optimization passes** (constant folding, fusion, quantization, partial evaluation),  
- **Backends** that lower operators to TVM for code generation.  

Key optimizations discussed in the paper include:  
- **Operator Fusion** – combines multiple operations into a single kernel for efficiency.  
- **Quantization Framework** – a generic rewriting approach that supports multiple quantization schemes, not just fixed ones.  
- **Partial Evaluation** – reduces overhead by evaluating static parts of the program early.  
- **Accelerator-Specific Optimizations** – such as convolution combination and layout transformations tailored for low-power and FPGA devices.  

## Evaluation
The experiments show Relay performs well on both **vision models** (ResNet, MobileNet, VGG) and **NLP models** (CharRNN, TreeLSTM, GRU, LSTM). Relay consistently achieves competitive or better performance compared to TensorFlow, PyTorch, and MXNet, particularly excelling in optimizations like fusion. It also demonstrates strong portability, compiling models to ARM CPUs and custom FPGA accelerators with effective quantization and layout strategies.  

## Limitations
- The paper focuses mainly on **inference**, not training.  
- Some NLP workloads benefit less due to missing hard-coded optimizations available in other frameworks.  
- Relay’s added expressivity increases compiler complexity, which may make some analyses or optimizations more challenging.  

## Summary
Relay shows that a **unified, functional IR** can achieve **expressivity, composability, portability, and competitive performance** in deep learning compilers. By combining programming language design with deep learning systems, Relay points toward a future where developers can more easily write flexible models and deploy them across diverse hardware without sacrificing efficiency.
