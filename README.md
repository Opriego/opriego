# Oscar Priego

**Senior Systems Software Engineer | Linux • Compilers • Performance • Open Source**

Systems software engineer with professional experience across Linux kernel debugging, low-level C/C++ development, CPU validation infrastructure, embedded systems, AI/GPU software infrastructure, and firmware tooling.

My current technical focus is the intersection of **Linux, compiler/toolchain engineering, hardware-aware optimization, and upstream open-source development**.

---

## Open Source & Upstream Work

### Linux Kernel

Working with the upstream Linux kernel contribution workflow, including:

* hardware and driver investigation
* HID subsystem debugging
* root-cause analysis from device and kernel behavior
* patch development
* `git format-patch` / `git send-email` workflow
* communication through Linux kernel mailing lists

Recent work includes an upstream HID patch for ELECOM hardware.

### LLVM / X86

**[llvm/llvm-project PR #216884 — Prefer VPMOVMSKB for nonnegative byte comparison masks](https://github.com/llvm/llvm-project/pull/216884)**

X86 backend optimization for signed byte comparison masks.

The change extends the existing MOVMSK lowering path for `v16i8` and `v32i8` comparisons so that non-negative sign tests can use `VPMOVMSKB` followed by `NOT` instead of generating an AVX-512 comparison into a mask register and transferring the result back to a GPR.

Work included:

* SelectionDAG / X86 lowering analysis
* instruction-selection investigation
* generated assembly comparison
* AVX2 and AVX-512 behavior
* LLVM CodeGen regression tests
* `FileCheck` validation
* upstream contribution workflow

**Fork:** [Opriego/llvm-project](https://github.com/Opriego/llvm-project)

---

## Compiler Engineering

### [Ocelotl Tensor Compiler](https://github.com/Opriego/ocelotl-tensor)

Experimental tensor-oriented compiler written in **C++20**, built as a practical compiler and systems engineering project.

Current pipeline:

```text
Source
  ↓
Lexer
  ↓
Parser
  ↓
AST
  ↓
Semantic Analysis
  ↓
Type & Shape Inference
  ↓
Ocelotl SSA-inspired IR
  ↓
LLVM IR
  ↓
Native Code
```

Implemented areas include:

* recursive-descent parsing
* source-aware diagnostics
* AST construction
* symbol resolution
* semantic analysis
* type inference
* tensor shape inference
* SSA-inspired intermediate representation
* LLVM-based code generation
* LLVM module verification
* command-line compiler frontend
* GoogleTest coverage
* GCC / Clang CI matrix
* CMake + Ninja
* Debian packaging with `debhelper`

Current backend work lowers scalar programs into valid LLVM IR, with tensor lowering and further CPU/GPU code generation forming the next major milestones.

---

## Systems / Automotive Open Source

### [ecutools](https://github.com/Opriego/ecutools)

Contributing fixes to an existing C-based automotive diagnostics and connectivity project involving CAN, J2534 and Linux/POSIX systems programming.

**[View my upstream pull requests](https://github.com/jeremyhahn/ecutools/pulls?q=is%3Apr+author%3AOpriego)**

Recent work includes:

* CAN frame comparison correctness
* socket bind failure handling
* shutdown semantics
* pointer/resource lifecycle fixes
* J2534 parameter validation
* transactional `PassThruOpen` / `PassThruClose` resource lifecycle
* unit-test coverage

The emphasis of these contributions is not only fixing individual defects, but improving **error handling, resource ownership, lifecycle correctness, testability, and failure-path behavior** in an existing C codebase.

---

## Systems Engineering Background

My professional work has included:

* Linux kernel and driver debugging
* low-level C and C++ development
* CPU architecture and platform validation
* large-scale hardware validation infrastructure
* power and uncore investigation
* firmware/debug tooling
* AI accelerator infrastructure
* GPU software enablement
* tensor and memory debugging
* performance-oriented problem solving
* production issue reproduction and root-cause analysis
* cross-functional technical communication

A recurring theme throughout my work is taking an initially ambiguous hardware/software problem and turning it into a **reproducible failure, evidence-based root cause, validated fix, and maintainable engineering solution**.

---

## Toolchains & Linux

Areas I actively work with include:

**Languages**

`C` · `C++` · `Python` · `LLVM IR` · `x86 Assembly`

**Compiler / Toolchain**

`LLVM` · `Clang` · `GCC` · `SelectionDAG` · `CMake` · `Ninja`

**Linux / Systems**

`Linux Kernel` · `GDB` · `Buildroot` · `Debian` · `POSIX` · `Git`

**Validation**

`GoogleTest` · `LLVM lit` · `FileCheck` · `GitHub Actions` · automated regression testing

**Systems domains**

CPU · GPU · AI accelerators · embedded systems · automotive · kernel/driver debugging

---

## What I'm Interested In

I am particularly interested in engineering work involving:

* Linux kernel development and debugging
* upstream open-source development
* compiler backends and optimization
* GCC / LLVM toolchains
* hardware/software interaction
* performance engineering
* platform enablement
* embedded Linux
* CPU/GPU architecture
* difficult cross-layer systems debugging

---

## Selected Repositories

* **[ocelotl-tensor](https://github.com/Opriego/ocelotl-tensor)** — C++20 compiler with custom IR and LLVM backend
* **[llvm-project](https://github.com/Opriego/llvm-project)** — LLVM upstream development fork
* **[ecutools](https://github.com/Opriego/ecutools)** — CAN/J2534/Linux systems contributions
* **[ocelotl_SGBDD](https://github.com/Opriego/ocelotl_SGBDD)** — earlier database/compiler-oriented academic project

---

## Contact

**GitHub:** [github.com/Opriego](https://github.com/Opriego)

Based in Guadalajara, Mexico. Open to remote and international systems/open-source engineering opportunities.

