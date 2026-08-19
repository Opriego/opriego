# Oscar Priego

**Senior Systems Software Engineer | Linux • LLVM • Compilers • Performance • Open Source**

Systems software engineer focused on low-level software, Linux, compiler/toolchain engineering, hardware-aware debugging, and open-source development.

My professional background spans **Linux kernel debugging, CPU validation infrastructure, embedded systems, AI accelerator/GPU software, firmware tooling, and C/C++ systems development**.

A recurring theme throughout my work is turning an initially ambiguous hardware/software problem into a **reproducible failure, evidence-based root cause, validated fix, and regression coverage**.

---

## Selected Open Source Work

### 🐧 Linux Kernel — HID

**[HID: elecom: fix bus type for M-XGL20DLBK](https://github.com/Opriego/linux/commit/ebdb82b3bd3c)**

Linux HID fix for an ELECOM device whose special-driver entry used the wrong HID bus matcher.

The device is handled as USB, but its entry in `hid_have_special_driver[]` used `HID_BLUETOOTH_DEVICE`, preventing the expected special-driver match.

The patch changes the entry to `HID_USB_DEVICE` and was prepared using the standard Linux kernel patch/submission workflow.

**Demonstrates:**

* Linux kernel source investigation
* HID/device matching
* root-cause analysis
* minimal corrective patches
* `Fixes:` / `Signed-off-by` conventions
* upstream mailing-list workflow

**Source branch:** [hid-elecom-m-xgl20dlbk-submitted](https://github.com/Opriego/linux/tree/hid-elecom-m-xgl20dlbk-submitted)

---

### ⚙️ LLVM / X86 Backend

**[llvm/llvm-project #216884 — Fold AVX-512 sign-mask compares to MOVMSK](https://github.com/llvm/llvm-project/pull/216884)**

Upstream LLVM X86 backend contribution targeting AVX-512 sign-mask generation.

The transformation recognizes complementary signed sign-test masks produced through `VPCMP*` + `KMOV` and folds appropriate 128-bit and 256-bit cases into the corresponding MOVMSK instruction:

```text
VPCMPB + KMOV  →  VPMOVMSKB
VPCMPD + KMOV  →  VMOVMSKPS
VPCMPQ + KMOV  →  VMOVMSKPD
```

with the required mask complement.

The implementation operates in `X86CompressEVEX` and includes checks for mask width, consumers, source clobbers, register constraints, and EFLAGS liveness.

Regression coverage includes both end-to-end CodeGen tests and dedicated MIR tests for positive and negative cases such as:

* multiple mask consumers
* narrowing `KMOV`
* source clobbers
* unsupported predicates/constants
* 512-bit vectors
* extended registers
* live EFLAGS
* incompatible masked-move consumers
* incorrect producer widths
* dead destinations

**Demonstrates:**

* LLVM backend development
* X86 instruction selection / machine-level optimization
* AVX2 / AVX-512 reasoning
* MIR and CodeGen testing
* liveness and register constraints
* assembly-level validation
* upstream review workflow

---

## Systems Programming in C

### 🔧 [c-systems-lab](https://github.com/Opriego/c-systems-lab)

A C17 systems-programming laboratory focused on making low-level engineering decisions explicit and testable.

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

Engineering validation includes:

* GCC and Clang strict-warning builds
* AddressSanitizer
* UndefinedBehaviorSanitizer
* ThreadSanitizer
* Valgrind integration
* `clang-tidy`
* `clang-format`
* CTest
* libFuzzer target
* GitHub Actions CI

The project emphasizes **explicit ownership, failure-path correctness, resource cleanup, bounded input handling, documented invariants, and predictable behavior under failure**.

---

## Compiler Engineering

### 🧠 [Ocelotl Tensor Compiler](https://github.com/Opriego/ocelotl-tensor)

Experimental compiler written in **C++20** for exploring an end-to-end compiler pipeline from source analysis through LLVM IR generation.

Current architecture:

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

Current functionality includes:

* recursive-descent parser
* source-aware diagnostics
* AST representation
* semantic analysis
* symbol resolution
* scalar type inference
* tensor shape inference
* `matmul` and `relu` semantic validation
* custom SSA-inspired IR
* LLVM backend for scalar programs
* LLVM module verification
* automated tests
* GCC / Clang CI
* CMake + Ninja
* Debian packaging
* command-line compiler frontend

The frontend and semantic model are intentionally separated from LLVM-specific lowering, providing a clean boundary between language semantics, intermediate representation, and backend code generation.

---

## Contributions to Existing Codebases

### 🚗 [ecutools](https://github.com/Opriego/ecutools)

Contributions to an existing C-based automotive diagnostics and connectivity codebase involving CAN, J2534, Linux/POSIX APIs, and AWS IoT integration.

**[View upstream pull requests](https://github.com/jeremyhahn/ecutools/pulls?q=is%3Apr+author%3AOpriego)**

Recent work includes:

* CAN frame comparison correctness
* socket bind failure handling
* shutdown semantics
* resource-lifecycle fixes
* NULL parameter validation
* transactional J2534 `PassThruOpen` / `PassThruClose` lifecycle
* unit-test coverage

The focus is not only on individual bugs, but on improving **resource ownership, failure paths, error propagation, lifecycle correctness, and testability** in an unfamiliar existing codebase.

---

## Historical Engineering Work

### 🗄️ [Ocelotl SGBDD](https://github.com/Opriego/ocelotl_SGBDD)

Historical distributed database management system originally developed in **2012**.

The project implemented:

* a hand-written SQL-like lexer and syntax analyzer
* token-stream processing
* distributed data dictionary
* query routing and rewriting
* heterogeneous database adapters
* node availability checks
* prototype coordinated commit/rollback
* GTK-based user interface

The repository is intentionally preserved as a historical artifact rather than rewritten as modern software.

It also documents some of my earliest practical work with concepts that later reappeared in compiler engineering: **lexical classification, token streams, syntax validation, language interpretation, and transformation of an input representation before execution**.

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
* performance-oriented investigation
* embedded Linux and cross-compilation
* production issue reproduction and root-cause analysis
* technical communication across engineering teams and stakeholders

I am particularly comfortable working where the initial report is incomplete and the problem crosses abstraction boundaries:

```text
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

---

## Engineering Approach

For difficult systems problems, I generally optimize for:

```text
Observe
  ↓
Reproduce
  ↓
Reduce
  ↓
Instrument
  ↓
Form hypotheses
  ↓
Falsify / confirm
  ↓
Root cause
  ↓
Minimal fix
  ↓
Regression coverage
  ↓
Document
```

I value fixes that explain **why the failure occurred**, preserve existing behavior outside the affected case, and leave behind enough test coverage and documentation to make the result maintainable.

---

## Technical Focus

**Languages**

`C` · `C++` · `Python` · `LLVM IR` · `x86 Assembly`

**Linux / Systems**

`Linux Kernel` · `POSIX` · `pthreads` · `GDB` · `Git` · `Debian` · `Buildroot`

**Compiler / Toolchain**

`LLVM` · `Clang` · `GCC` · `LLVM MIR` · `CMake` · `Ninja`

**Validation / Quality**

`LLVM lit` · `FileCheck` · `GoogleTest` · `CTest` · `ASan` · `UBSan` · `TSan` · `Valgrind` · `clang-tidy` · `GitHub Actions`

**Domains**

Kernel / drivers · CPU · GPU · AI accelerators · compiler backends · embedded systems · automotive · performance engineering

---

## Selected Repositories

| Project                                                         | Focus                                           |
| --------------------------------------------------------------- | ----------------------------------------------- |
| **[linux](https://github.com/Opriego/linux)**                   | Linux kernel / upstream patch development       |
| **[llvm-project](https://github.com/Opriego/llvm-project)**     | LLVM X86 backend contribution                   |
| **[ocelotl-tensor](https://github.com/Opriego/ocelotl-tensor)** | C++20 compiler / SSA IR / LLVM backend          |
| **[c-systems-lab](https://github.com/Opriego/c-systems-lab)**   | C17 / POSIX / concurrency / networking / memory |
| **[ecutools](https://github.com/Opriego/ecutools)**             | Existing C codebase / CAN / J2534 / Linux       |
| **[ocelotl_SGBDD](https://github.com/Opriego/ocelotl_SGBDD)**   | Historical parser / distributed systems project |

---

## Current Interests

I am particularly interested in engineering roles involving:

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

**GitHub:** [github.com/Opriego](https://github.com/Opriego)
**Location:** Guadalajara, Mexico

Open to remote and international systems/open-source engineering opportunities.

