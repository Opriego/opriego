# Oscar Priego Verdugo

### Senior Systems Software Engineer
**LLVM · Compilers · Linux · GPU · Performance · Open Source**

Senior systems software engineer with ~14 years of experience working close to the hardware/software boundary.

I specialize in turning ambiguous low-level failures into reproducible cases, root causes, minimal fixes, and regression coverage.

My current focus is **compiler engineering and upstream LLVM development**, building on a background in Linux systems, CPU validation, firmware tooling, performance analysis, and low-level C/C++ engineering.

> **Open to senior compiler, systems software, GPU compiler, performance, and open-source engineering opportunities.**
>
> Particularly interested in teams working on **LLVM, GCC, GPU compilers, Linux, heterogeneous computing, and low-level systems software**.

---

## LLVM Upstream

I actively contribute to [`llvm/llvm-project`](https://github.com/llvm/llvm-project), with multiple production changes already merged upstream across compiler analysis and code generation.

My work currently spans:

- **ValueTracking / compiler analysis**
- **X86 backend**
- **SelectionDAG**
- **Vector legalization**
- **SIMD and vectorization**
- **Floating-point correctness**
- **Code-generation optimization**
- **Crash-on-valid debugging and reduction**

### Merged upstream

| Area | Contribution | Status |
| --- | --- | --- |
| ValueTracking | `llvm.stepvector` known-bits reasoning | **Merged to `main`** |
| X86 | Non-negative byte-test recognition | **Merged to `main`** |
| X86 | `minimumnum` / `maximumnum` NaN-sign lowering | **Merged to `main`** |
| SelectionDAG | `CTTZ_ELTS` legalization / widening correctness | **Merged to `main`** |

These are production LLVM changes developed and reviewed through the normal upstream process.

### Selected LLVM work

#### ValueTracking — `llvm.stepvector` known bits

Extended `computeKnownBits()` reasoning for `llvm.stepvector`, including scalable-vector cases constrained by `vscale_range`.

The additional information enables downstream optimizations to prove properties such as `nuw`, `nsw`, and disjoint bitwise operations that could not previously be established.

The change was reviewed upstream in the ValueTracking area and merged into LLVM `main`.

#### X86 — `minimumnum` / `maximumnum` lowering

Investigated X86 lowering around NaN-sign semantics and implemented target-specific handling while preserving the required floating-point behavior.

The resulting code generation substantially reduced instruction counts across several AVX and AVX-512 cases while preserving NaN and signed-zero semantics.

#### SelectionDAG — vector legalization

Investigated failures involving `llvm.experimental.cttz.elts` and vector type legalization.

Rather than treating the final backend failure as the root cause, I traced the behavior through SelectionDAG legalization and expansion, produced reduced IR reproducers, validated the fix with assertion-enabled LLVM builds, and added targeted regression coverage.

### Current upstream work

I continue to investigate LLVM issues beyond the areas where I already have merged changes.

Current work includes `callbr` / `INLINEASM_BR` output handling in SelectionDAG, where COPY provenance must survive indirect-destination lowering for later landing-pad reconstruction.

I am deliberately building breadth across LLVM's analysis, optimization, and code-generation pipeline rather than concentrating exclusively on one class of fixes.

---

## What I Bring to Compiler Engineering

My background was not limited to compilers.

I came into compiler development after years of working across systems where failures frequently cross abstraction boundaries:

- Linux systems and kernel-level debugging
- CPU validation and test infrastructure
- Hardware/software failure analysis
- Firmware and platform tooling
- Performance-sensitive C/C++
- Python automation and infrastructure
- Low-level debugging
- Reproduction and reduction of complex failures

That background affects how I approach compiler bugs.

When generated code is wrong, slow, or crashes, I do not automatically assume the backend is responsible. I follow the evidence through the complete stack.

    Application
        ↓
    Runtime / libraries
        ↓
    Frontend / IR
        ↓
    Compiler analysis
        ↓
    Optimization
        ↓
    Instruction selection
        ↓
    ABI / calling convention
        ↓
    Kernel
        ↓
    Firmware
        ↓
    Hardware

The layer where a failure becomes visible is not necessarily the layer where the bug originated.

---

## Engineering Approach

My debugging workflow is evidence-driven:

    Observe
       ↓
    Reproduce
       ↓
    Reduce
       ↓
    Instrument
       ↓
    Form competing hypotheses
       ↓
    Disprove them
       ↓
    Find the root cause
       ↓
    Implement the smallest correct fix
       ↓
    Add regression coverage
       ↓
    Upstream review

I value **small, explainable changes backed by strong evidence** over large speculative fixes.

A good fix should answer three questions:

1. **Why did the original behavior fail?**
2. **Why does this change correct the underlying problem?**
3. **What prevents the same class of failure from returning?**

---

## Technical Focus

### Compilers

- LLVM
- LLVM IR
- ValueTracking / KnownBits
- SelectionDAG
- X86 code generation
- RISC-V
- SIMD and vectorization
- Vector legalization
- Floating-point semantics
- Optimization and legality reasoning
- ABI-sensitive lowering

### Systems

- C / C++
- Linux
- Kernel-level debugging
- CPU architecture
- Performance analysis
- Firmware interaction
- Hardware/software debugging
- Assembly
- GDB / LLDB
- `perf`

### GPU & Heterogeneous Computing

- GPU compiler architecture
- Parallel execution models
- Tensor computation
- Hardware-aware optimization
- Compiler/runtime interaction
- Alternative execution models

### Engineering Infrastructure

- Python
- CMake
- Ninja
- Git
- CI / regression testing
- FileCheck
- llvm-lit
- Reproducer construction and reduction

---

## Independent Research & Projects

Alongside upstream work, I build experimental compiler and systems projects to explore ideas from first principles.

My current research interests include:

- Tensor semantics
- Parallel execution models
- Responsibility-based work decomposition
- Compiler transformation synthesis
- Semantic verification
- GPU-oriented execution
- Deterministic exploration of implementation strategies
- Systems and game-engine workloads as practical execution-model experiments

I treat these projects as **engineering laboratories** rather than demonstrations: ideas are expected to survive implementation, testing, counterexamples, and measurement.

---

## Selected Repositories

### [`llvm-project`](https://github.com/llvm/llvm-project)

My upstream LLVM contribution work.

Contributions include compiler analysis, code-generation optimization, correctness fixes, SelectionDAG work, and regression coverage.

### Linux / Kernel Work

Low-level Linux and systems work involving kernel behavior, hardware/software interaction, debugging, and performance analysis.

### Compiler & Systems Experiments

My other repositories contain C/C++, compiler, systems, tensor, and hardware/software experiments developed while investigating lower-level engineering problems.

---

## Languages & Tools

**Languages**

`C` · `C++` · `Python` · `LLVM IR` · `Assembly`

**Compiler ecosystem**

`LLVM` · `Clang` · `FileCheck` · `llvm-lit`

**Systems**

`Linux` · `GDB` · `LLDB` · `perf` · `objdump`

**Development**

`Git` · `CMake` · `Ninja`

**Architecture**

`x86-64` · `SIMD` · `RISC-V` · `GPU`

---

## What I Like Working On

The problems I enjoy most usually begin with:

> **"This should work, but it doesn't — and nobody is quite sure why."**

Compiler correctness bugs, optimizer corner cases, assertion failures, performance anomalies, ABI interactions, hardware/software boundary failures, and problems that require reducing a large system into one precise explanation are the kind of engineering work I gravitate toward.

I am especially interested in roles where **deep debugging, compiler internals, performance, and hardware awareness intersect**.

---

## Open to Opportunities

I am currently open to conversations about:

- **Senior / Staff Compiler Engineering**
- **LLVM / GCC development**
- **GPU Compiler Engineering**
- **Systems Software Engineering**
- **Linux / low-level platform engineering**
- **Performance engineering**
- **Open-source engineering**

I am particularly interested in organizations where upstream open-source development is part of the engineering culture.

Remote and international opportunities are welcome.

---

## Contact & Links

- **GitHub:** [github.com/Opriego](https://github.com/Opriego)
- **LLVM contributions:** [Pull requests by Opriego](https://github.com/llvm/llvm-project/pulls?q=is%3Apr+author%3AOpriego)
- **LinkedIn:** Add your LinkedIn URL here
- **Email:** Add your preferred professional email here

---

### Observe · Reproduce · Reduce · Understand · Fix · Verify · Upstream
