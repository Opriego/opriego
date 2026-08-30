# Oscar Priego

### Senior Systems Software Engineer

**Compilers · LLVM · Linux · GPU · Performance · Low-Level Systems · Open Source**

Systems software engineer with ~14 years of experience working across the hardware/software boundary.

My background spans **compiler and toolchain engineering, Linux kernel and driver debugging, CPU/platform validation, embedded systems, AI accelerator and GPU software, firmware tooling, performance analysis, and C/C++ systems development**.

A recurring theme throughout my work is taking an ambiguous low-level problem and turning it into a **reproducible failure, evidence-based root cause, minimal fix, and regression test**.

> **Open to remote and international opportunities in compiler, GPU, systems software, performance, and open-source engineering.**

---

## Open Source Upstream

I contribute fixes and improvements to large existing codebases, with an emphasis on understanding the underlying invariants rather than patching symptoms.

### LLVM

Active contributor to [`llvm/llvm-project`](https://github.com/llvm/llvm-project), with **four patches merged to `main`** across compiler analysis and code generation.

My upstream work spans:

* ValueTracking and KnownBits
* X86 backend optimization
* floating-point correctness
* SelectionDAG
* vector type legalization
* crash-on-valid debugging
* SIMD and code-generation analysis

#### Selected contributions

**ValueTracking — `llvm.stepvector` known-bits reasoning**

Extended `computeKnownBits()` reasoning for `llvm.stepvector`, including scalable vectors constrained by `vscale_range`.

The additional information enables downstream transformations to prove properties such as `nuw`, `nsw`, and disjoint bitwise operations.

Reviewed upstream in the ValueTracking area and merged to `main`.

**X86 — non-negative byte-test recognition**

Improved X86 code generation for signed byte-test masks, replacing eligible AVX/AVX-512 compare-and-mask sequences with MOVMSK-based lowering while accounting for register constraints, liveness, consumers, and mask semantics.

Regression coverage includes end-to-end CodeGen and focused MIR cases.

**X86 — `minimumnum` / `maximumnum` NaN handling**

Investigated X86 lowering around NaN and signed-zero semantics and compared the target-specific implementation against LLVM's generic expansion.

Representative measurements showed substantial reductions in generated instruction count while preserving required bit-level semantics:

| Case                    | X86 lowering | Generic expansion |
| ----------------------- | -----------: | ----------------: |
| scalar `f32`, AVX       |            7 |                13 |
| `<4 x float>`           |            7 |                58 |
| `<8 x float>`           |            7 |               118 |
| `<16 x float>`, AVX-512 |            7 |               272 |

**SelectionDAG — `CTTZ_ELTS` legalization**

Investigated correctness and crash failures involving `llvm.experimental.cttz.elts`.

One issue involved confusing a vector's **logical lane count** with the width of a helper vector after target legalization. A related investigation exposed an assertion involving irregular fixed-length `i1` vectors and `INSERT_SUBVECTOR`.

The work involved reduced IR reproducers, assertion-enabled builds, SelectionDAG legalization analysis, cross-target validation, and regression coverage.

#### Current LLVM work

I continue to work across LLVM rather than restricting contributions to a single backend or subsystem.

Current investigations include `callbr` / `INLINEASM_BR` handling in SelectionDAG, vectorization, RISC-V, and other correctness and optimization issues.

**LLVM contributions:**
[Pull requests by Opriego](https://github.com/llvm/llvm-project/pulls?q=is%3Apr+author%3AOpriego)

---

### Linux Kernel

I have also worked through the Linux kernel upstream development workflow.

**HID: ELECOM device matching**

Investigated an ELECOM device whose special-driver entry used the wrong HID bus matcher.

The device operated over USB while the corresponding entry used the Bluetooth matcher, preventing the expected driver selection.

The fix changes the entry to the correct USB matcher and was prepared through the standard kernel patch/submission workflow.

**Areas:** kernel source investigation · HID · device matching · minimal fixes · upstream workflow

---

## Compiler Engineering

### Ocelotl Tensor Compiler

[`ocelotl-tensor`](https://github.com/Opriego/ocelotl-tensor) is an experimental C++20 compiler exploring an end-to-end compilation pipeline:

```
Source
  ↓
Lexer
  ↓
Parser
  ↓
AST
  ↓
Semantic Analysis
  ├─ symbol resolution
  ├─ type inference
  ├─ shape inference
  └─ validation
  ↓
Ocelotl IR
(SSA-inspired)
  ↓
LLVM Lowering
  ↓
llvm::Module
  ↓
LLVM Verifier
  ↓
LLVM IR
```

Implemented functionality includes:

* recursive-descent parsing
* source-aware diagnostics
* AST representation
* semantic analysis
* symbol resolution
* scalar type inference
* tensor shape inference
* `matmul` and `relu` semantic validation
* custom SSA-inspired IR
* LLVM backend
* LLVM module verification
* automated testing
* GCC / Clang CI
* CMake + Ninja
* Debian packaging
* command-line compiler frontend

The architecture deliberately separates **language semantics, intermediate representation, and LLVM-specific lowering**.

This project complements my LLVM upstream work: one exercises compiler construction from the frontend downward, while the other requires reasoning inside a large production compiler infrastructure.

---

## Systems Programming

### c-systems-lab

[`c-systems-lab`](https://github.com/Opriego/c-systems-lab) is a C17 systems-programming laboratory focused on explicit ownership, failure-path correctness, resource lifecycle, concurrency, networking, and testability.

Implemented components include:

* arena allocator with alignment and overflow protection
* failure-safe dynamic vector
* preallocated ring buffer
* validated binary protocol codec
* endian-aware framing and checksums
* robust POSIX file-descriptor I/O
* partial-read/write and `EINTR` handling
* `fork` / `exec` / `dup2` / pipes / `waitpid`
* bounded pthread thread pool
* IPv4/IPv6 TCP networking
* integrated systems demo server

Validation includes:

`GCC` · `Clang` · `ASan` · `UBSan` · `TSan` · `Valgrind` · `libFuzzer` · `clang-tidy` · `CTest` · `GitHub Actions`

---

## Contributions to Existing Codebases

### ecutools

Contributions to [`ecutools`](https://github.com/Opriego/ecutools), an existing C automotive diagnostics/connectivity codebase involving CAN, J2534, Linux/POSIX APIs, and IoT integration.

Work includes:

* CAN frame comparison correctness
* socket bind failure handling
* shutdown semantics
* resource-lifecycle fixes
* NULL parameter validation
* transactional J2534 `PassThruOpen` / `PassThruClose` lifecycle
* unit-test integration

This work follows the same pattern as my upstream contributions: **enter an unfamiliar codebase, identify its ownership and behavioral invariants, make the smallest defensible change, and leave regression coverage behind**.

---

## Systems Engineering Background

My broader engineering experience includes:

* Linux kernel and driver debugging
* C and C++ systems programming
* CPU/platform validation
* large-scale hardware validation infrastructure
* CPU power and uncore investigation
* firmware and debugging tooling
* AI accelerator infrastructure
* GPU software enablement
* tensor and memory debugging
* performance investigation
* embedded Linux and cross-compilation
* production issue reproduction and root-cause analysis

I am particularly comfortable with failures that cross abstraction boundaries:

```
Application
    ↓
Runtime / libraries
    ↓
Compiler / generated code
    ↓
Kernel / driver
    ↓
Firmware
    ↓
Hardware
```

The layer where a problem becomes visible is not necessarily the layer where it originated.

---

## Engineering Approach

For difficult compiler and systems problems, I generally work through:

```
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
Falsify / confirm
  ↓
Root cause
  ↓
Minimal fix
  ↓
Regression coverage
  ↓
Validate
  ↓
Upstream / document
```

I value fixes that:

1. explain **why** the failure occurred;
2. preserve behavior outside the affected case;
3. minimize the blast radius;
4. are backed by reproducible evidence; and
5. leave enough regression coverage to prevent recurrence.

When an implementation choice is questioned during review, I prefer reducing the disagreement to something measurable: **semantics, generated code, instruction counts, target behavior, or a reproducible test**.

---

## Technical Focus

| Area                       | Technologies / Domains                                                              |
| -------------------------- | ----------------------------------------------------------------------------------- |
| **Languages**              | C · C++ · Python · LLVM IR · x86 Assembly                                           |
| **Compiler / Toolchain**   | LLVM · Clang · SelectionDAG · ValueTracking · LLVM MIR · FileCheck · llvm-lit · GCC |
| **Linux / Systems**        | Linux Kernel · POSIX · pthreads · GDB · Buildroot · Debian                          |
| **Build / Engineering**    | Git · CMake · Ninja · CI · automated testing                                        |
| **Validation**             | GoogleTest · CTest · ASan · UBSan · TSan · Valgrind · libFuzzer · clang-tidy        |
| **Architecture / Domains** | x86 · RISC-V · CPU · GPU · AI accelerators · embedded systems · performance         |

---

## Selected Repositories

| Project                                                       | Focus                                                                   |
| ------------------------------------------------------------- | ----------------------------------------------------------------------- |
| [`llvm-project`](https://github.com/Opriego/llvm-project)     | LLVM upstream compiler analysis, CodeGen, SelectionDAG and optimization |
| [`ocelotl-tensor`](https://github.com/Opriego/ocelotl-tensor) | C++20 compiler · frontend · SSA-inspired IR · LLVM backend              |
| [`linux`](https://github.com/Opriego/linux)                   | Linux kernel · upstream patch development                               |
| [`c-systems-lab`](https://github.com/Opriego/c-systems-lab)   | C17 · POSIX · memory · processes · concurrency · networking             |
| [`ecutools`](https://github.com/Opriego/ecutools)             | Existing C codebase · CAN · J2534 · Linux/POSIX                         |
| [`ocelotl_SGBDD`](https://github.com/Opriego/ocelotl_SGBDD)   | Historical parser / distributed database systems work                   |

---

## Current Direction

I am particularly interested in engineering work involving:

* LLVM and GCC
* compiler analysis and optimization
* compiler backends and instruction selection
* GPU and heterogeneous compilation
* CPU/GPU architecture
* AI/ML compiler infrastructure
* Linux and low-level systems
* performance engineering
* upstream open-source development
* difficult cross-layer debugging

My current direction is deeper compiler and GPU work, but I approach those problems with the perspective of a systems engineer who is comfortable following failures all the way from **source code to generated instructions to kernel, firmware, and hardware behavior**.

---

## Open to Opportunities

I am open to **Senior / Staff-level** conversations involving:

* Compiler Engineering
* LLVM / GCC
* GPU Compiler Engineering
* Systems Software Engineering
* Performance Engineering
* Linux / Platform Engineering
* Open-Source Engineering

**Remote and international opportunities are welcome.**

---

### Observe · Reproduce · Reduce · Understand · Fix · Verify · Upstream
