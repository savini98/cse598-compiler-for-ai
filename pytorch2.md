# Review of *PyTorch 2: Faster Machine Learning Through Dynamic Python Bytecode Transformation and Graph Compilation*

The paper introduces **TorchDynamo** and **TorchInductor**, two
extensions behind the new `torch.compile` feature in **PyTorch 2**.
These tools address the long-standing tension between the **flexibility
of eager execution** and the **performance benefits of graph
compilation**. The authors show how dynamic Python bytecode
transformation and graph-based compilation can deliver significant
speedups without forcing users to abandon Python's programming model.

In the **introduction**, the paper highlights the trade-offs between
eager-mode frameworks like PyTorch and graph-mode frameworks such as
TensorFlow. While eager execution is preferred for its usability and
debuggability, it prevents global compiler optimizations. Prior attempts
at graph capture in PyTorch (e.g., TorchScript, Lazy Tensors, FX
tracing) either suffered from unsoundness, limited coverage, or high
runtime overhead. TorchDynamo and TorchInductor aim to overcome these
limitations.

The **TorchDynamo framework** is presented as a **Python-level JIT
compiler** that hooks into CPython's frame evaluation API. It
dynamically rewrites Python bytecode to extract PyTorch operations into
**FX graphs**, mixing compiled fragments with regular Python execution
to retain flexibility. TorchDynamo uses **guards** to ensure correctness
under dynamic conditions, supports control flow and closures, and can
gracefully fall back when unsupported constructs arise. For training, it
integrates with **AOTAutograd** to generate forward and backward graphs.

The **TorchInductor compiler** is described as the default backend for
TorchDynamo. It compiles FX graphs into efficient code: **Triton kernels
for GPUs** and **C++/OpenMP for CPUs**. Its design emphasizes (1)
maintaining PyTorch's semantics (e.g., support for strides, in-place
mutation), (2) being Python-first and hackable, (3) supporting a broad
set of operators, and (4) reusing existing high-performance DSLs.
TorchInductor employs decompositions, a loop-level **define-by-run IR**,
scheduling and fusion strategies, and code generation targeting both
Triton and C++. It also supports **dynamic shapes**, using symbolic
guards and meta functions to avoid recompilation on shape variation.

The **experiments** span three benchmark suites: TorchBench, HuggingFace
Transformers, and TIMM vision models. Results show:\
- **TorchDynamo** successfully captures graphs for nearly all models
(93--100% coverage vs. \~50% for TorchScript).\
- Graph capture overhead is minimal (\<5%), significantly lower than
Lazy Tensors.\
- **TorchInductor** delivers up to **2.27× inference speedup** and
**1.41× training speedup** (geometric mean) on an NVIDIA A100 across
180+ real-world models, outperforming six other compiler backends.

The **results** demonstrate that PyTorch 2 achieves strong speedups
while preserving Python's flexibility. TorchDynamo captures complex
models robustly, and TorchInductor generates competitive kernels,
rivaling or surpassing state-of-the-art compilers. Importantly, the
implementation is **open source** and integrated into PyTorch's nightly
builds, enabling reproducibility and broad adoption.

In the **conclusion**, the authors argue that `torch.compile` represents
a major step forward in marrying the ease of eager execution with the
efficiency of compiler optimizations. By bridging dynamic Python with
graph-based compilation, PyTorch 2 empowers researchers and
practitioners to achieve **faster machine learning** without sacrificing
usability or ecosystem compatibility.
