---
layout: page
title: code.GAP
description: "Experimenting with GEMV acceleration — from CPU baselines to hardware co-design."
img: assets/img/12.jpg # 프로젝트 카드에 표시될 배경 이미지
importance: 1
category: Research
---

<div class="row">
    <div class="col-sm-12">
        {% include figure.liquid loading="eager" path="assets/img/gap_overview.jpg" title="Project Overview Image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    A conceptual diagram of the code.GAP project, showing the progression from software kernels to an FPGA-based hardware accelerator. 
    </div>

## **Overview**

`code.GAP` is a **practice-oriented project** for exploring **GEMV (General Matrix-Vector Multiplication)** acceleration. It is designed both as a **learning companion for computer architecture courses** and as a **foundation for accelerator co-design experiments** involving quantization, sparsity, and custom hardware.

The project focuses on:
- Building a reproducible **software/hardware stack** for performance analysis based on the Roofline model.
- Evaluating the impact of modern acceleration techniques like **quantization** (INT8) and **structured sparsity** (2:4).
- Understanding design trade-offs between **CPU baselines, tiled kernels, and systolic-array models**.
- Producing benchmarks and correctness tests for **portfolio and research use**.

---

## **Layered Architecture**

The project is designed around a "Contract"-based architecture to ensure a clean separation of concerns between layers. This modular design allows for independent development and testing of the tensor library, backend implementations, and GEMV algorithms.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/gap_architecture.jpg" title="Layered Architecture Diagram" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        <h3 class="mt-0">Core Layers</h3>
        <ul>
            <li><strong>Core:</strong> Defines the central contracts (data types, compute policies) and the abstract `IBackend` interface.</li>
            <li><strong>Tensor:</strong> A user-friendly handle for managing data and metadata.</li>
            <li><strong>Backend:</strong> A resource provider for memory operations and optional accelerated kernels (e.g., NEON, AVX, FPGA).</li>
            <li><strong>Algorithm:</strong> The GEMV dispatcher that selects an appropriate backend or falls back to a reference software kernel.</li>
        </ul>
    </div>
</div>
<div class="caption">
    A visual representation of the project's layered architecture, highlighting the separation between interfaces and implementations.
</div>


---

## **Project Roadmap**

The project is structured in four sequential phases, progressing from fundamental software concepts to a final hardware implementation.

1.  **Phase 1: Software Kernels (Practice)**
    - Implement baseline GEMV kernels (Naive, Tiled/Blocking) in pure software to establish correctness and a performance reference.

2.  **Phase 2: CPU Optimization**
    - Develop CPU-specific backends using SIMD intrinsics (Arm NEON, x86 AVX) to optimize the software kernels.

3.  **Phase 3: Accelerator Modeling**
    - Build a cycle-approximate software model of a systolic array to understand its dataflow and performance characteristics without designing hardware.

4.  **Phase 4: FPGA Co-Design (Hardware)**
    - Design, implement, and verify a complete **INT8, 8x8 Output-Stationary Systolic Array** accelerator on an FPGA, managed by an AXI-based host interface.

---

## **Build & Run**

The project includes a simple build script and a command-line benchmark runner for standardized testing.

Below is an example of running an INT8 benchmark with 2:4 structured sparsity.

{% raw %}
```bash
# Build the project (default: cpu.scalar backend)
bash scripts/build.sh

# Run an INT8 benchmark with 2:4 structured sparsity
./build/gap_app --backend=cpu.scalar --kernel=sw.tiled \
    --dtype=i8 --math=Int8xInt8_To_Int32 --sparsity=block2_4 \
    --M=4096 --N=4096 --runs=30 --warmup=5
```
{% endraw %}

<div class="text-center mt-4">
<a class="btn btn-primary btn-lg" href="https://github.com/code0-god/code.GAP" role="button" target="_blank">
<i class="fab fa-github"></i> View on GitHub
</a>
</div>