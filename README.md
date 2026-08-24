# Oscar Priego

**Senior Systems Software Engineer | LLVM · Compilers · Linux · GPU · Performance · Open Source**

Systems software engineer focused on compiler/toolchain engineering, low-level software, Linux, hardware-aware debugging, and performance.

My background spans Linux kernel and driver debugging, CPU/platform validation, embedded systems, AI accelerator and GPU software, firmware tooling, and C/C++ systems development.

A recurring theme throughout my work is taking an initially ambiguous hardware/software problem and turning it into a **reproducible failure, evidence-based root cause, validated fix, and regression test**.

I am currently contributing upstream to **LLVM**, with work spanning the **X86 backend and generic SelectionDAG legalization/expansion**.

---

## LLVM Upstream Work

Recent contributions to [`llvm/llvm-project`](https://github.com/llvm/llvm-project) include backend optimization, floating-point correctness, SelectionDAG expansion, and vector type legalization.

### [#216884 — X86: Fold AVX-512 sign-mask compares to MOVMSK](https://github.com/llvm/llvm-project/pull/216884)

Machine-level X86 optimization in `X86CompressEVEX`.

Recognizes complementary signed sign-test masks implemented through AVX-512 `VPCMP* + KMOV` sequences and folds eligible cases to the corresponding MOVMSK instruction:

```text
VPCMPB + KMOV  →  VPMOVMSKB
VPCMPD + KMOV  →  VMOVMSKPS
VPCMPQ + KMOV  →  VMOVMSKPD
```

The transformation accounts for:

* mask width and polarity
* multiple consumers
* narrowing `KMOV`
* source clobbers
* register constraints
* extended registers
* EFLAGS liveness
* incompatible masked-move consumers
* dead destinations

Regression coverage includes both end-to-end CodeGen tests and focused MIR positive/negative cases.

**Areas:** X86 backend · AVX2/AVX-512 · MIR · machine-level optimization · register/liveness reasoning

---

### [#217420 — X86: Fix NaN handling in minimumnum/maximumnum zero fixup](https://github.com/llvm/llvm-project/pull/217420)

Correctness fix for X86 lowering of LLVM `minimumnum` / `maximumnum` operations involving NaNs and signed zero.

The investigation covered:

* qNaN and sNaN behavior
* operand ordering
* signed-zero semantics
* scalar and vector lowering
* SSE / AVX / AVX-512 configurations
* native AVX10.2 behavior
* fast-math interactions

When the existing custom lowering was questioned against LLVM's generic expansion, I compared both implementations experimentally.

Representative instruction-count results:

| Case                    | X86 custom lowering | Generic expansion |
| ----------------------- | ------------------: | ----------------: |
| scalar `f32`, AVX       |                   7 |                13 |
| `<4 x float>`           |                   7 |                58 |
| `<8 x float>`           |                   7 |               118 |
| `<16 x float>`, AVX-512 |                   7 |               272 |

The custom lowering preserved the required bit-level NaN and signed-zero behavior while producing substantially better vector code.

**Upstream review:** approved / LGTM.

**Areas:** X86 lowering · IEEE-754 semantics · NaNs · signed zero · codegen analysis · experimental validation

---

### [#217982 — SelectionDAG: Preserve CTTZ_ELTS lane count during expansion](https://github.com/llvm/llvm-project/pull/217982)

Correctness fix in generic SelectionDAG expansion.

`expandCttzElts()` derived its logical vector length from an auxiliary step vector after target legalization. On X86 with AVX-512F, a semantic `<4 x i1>` operation could use a helper vector widened from `v4i8` to `v16i8`, causing an all-zero `CTTZ_ELTS` input to return **16 instead of 4**.

The fix preserves the original `ElementCount` from the `CTTZ_ELTS` operand and uses that logical lane count independently of the helper vector's legalized representation.

Regression coverage includes affected `v4i1` and `v8i1` AVX-512 cases.

**Areas:** SelectionDAG · legalization · vector semantics · `ElementCount` · target-independent compiler correctness

---

### [#218019 — SelectionDAG: Avoid irregular INSERT_SUBVECTOR when widening CTTZ_ELTS](https://github.com/llvm/llvm-project/pull/218019)

Follow-up investigation into `CTTZ_ELTS` exposed a separate crash involving irregular fixed-length `i1` vectors.

The failure path involved:

```text
CTTZ_ELTS
    ↓
operand widening
    ↓
INSERT_SUBVECTOR
    ↓
vector result splitting
    ↓
SplitVecRes_INSERT_SUBVECTOR
    ↓
getVectorSubVecPointer
    ↓
attempt to byte-address i1 elements
    ↓
assertion
```

I reduced and reported the failure as:

[#217985 — SplitVecRes_INSERT_SUBVECTOR asserts for irregular i1 vectors](https://github.com/llvm/llvm-project/issues/217985)

The proposed fix uses `VECTOR_SHUFFLE` for the affected widening case, selecting original lanes from the widened source and padding lanes from an all-ones vector. This avoids the problematic irregular `INSERT_SUBVECTOR` while preserving the required non-poison semantics.

Validation covers X86 and relevant target-independent behavior.

**Areas:** SelectionDAG · type legalization · irregular vectors · `i1` · crash debugging · compiler invariants

---

## Linux Kernel Upstream Work

### [HID: elecom: fix bus type for M-XGL20DLBK](https://github.com/Opriego/linux/tree/hid-elecom-m-xgl20dlbk-submitted)

Linux HID fix for an ELECOM device whose special-driver entry used the wrong HID bus matcher.

The device is handled as USB, but its entry in `hid_have_special_driver[]` used `HID_BLUETOOTH_DEVICE`, preventing the expected special-driver match.

The patch changes the entry to `HID_USB_DEVICE` and was prepared using the standard Linux kernel patch/submission workflow.

**Areas:** kernel source investigation · HID/device matching · minimal fixes · mailing-list workflow

---

## Compiler Engineering

### [Ocelotl Tensor Compiler](https://github.com/Opriego/ocelotl-tensor)

Experimental C++20 compiler exploring an end-to-end pipeline from source analysis through LLVM IR generation.

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

* recursive-descent parsing
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
* automated testing
* GCC / Clang CI
* CMake + Ninja
* Debian packaging
* command-line compiler frontend

The frontend and semantic model are intentionally separated from LLVM-specific lowering, keeping language semantics, intermediate representation, and backend code generation as distinct layers.

---

## Systems Programming

### [c-systems-lab](https://github.com/Opriego/c-systems-lab)

C17 systems-programming laboratory focused on explicit ownership, failure-path correctness, resource cleanup, concurrency, and testability.

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

* strict GCC and Clang builds
* AddressSanitizer
* UndefinedBehaviorSanitizer
* ThreadSanitizer
* Valgrind
* `clang-tidy`
* `clang-format`
* CTest
* libFuzzer
* GitHub Actions

---

## Contributions to Existing Codebases

### [ecutools](https://github.com/Opriego/ecutools)

Contributions to an existing C automotive diagnostics/connectivity codebase involving CAN, J2534, Linux/POSIX APIs, and AWS IoT integration.

Recent work includes:

* CAN frame comparison correctness
* socket bind failure handling
* shutdown semantics
* resource-lifecycle fixes
* NULL parameter validation
* transactional J2534 `PassThruOpen` / `PassThruClose` lifecycle
* unit-test integration

The emphasis is on understanding unfamiliar code, identifying ownership and lifecycle invariants, producing minimal fixes, and leaving behind regression coverage.

---

## Systems Engineering Background

My broader engineering experience includes work across:

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
* cross-team technical communication

I am particularly comfortable with problems that cross abstraction boundaries:

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

For difficult systems and compiler problems, I generally work through:

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

I value fixes that explain **why** a failure occurred, preserve behavior outside the affected case, and leave enough regression coverage to make the result maintainable.

Upstream review is part of that process: if an implementation choice is questioned, I prefer to reduce the disagreement to something measurable — semantics, generated code, instruction counts, target behavior, or a reproducible test.

---

## Technical Focus

**Languages**

`C` · `C++` · `Python` · `LLVM IR` · `x86 Assembly`

**Compiler / Toolchain**

`LLVM` · `Clang` · `SelectionDAG` · `LLVM MIR` · `FileCheck` · `LLVM lit` · `GCC` · `CMake` · `Ninja`

**Linux / Systems**

`Linux Kernel` · `POSIX` · `pthreads` · `GDB` · `Git` · `Debian` · `Buildroot`

**Validation / Quality**

`GoogleTest` · `CTest` · `ASan` · `UBSan` · `TSan` · `Valgrind` · `clang-tidy` · `GitHub Actions`

**Domains**

Compiler backends · CPU · GPU · AI accelerators · Linux kernel/drivers · embedded systems · performance engineering · hardware/software interaction

---

## Selected Repositories

| Project                                                     | Focus                                                  |
| ----------------------------------------------------------- | ------------------------------------------------------ |
| [llvm-project](https://github.com/Opriego/llvm-project)     | LLVM X86 backend / SelectionDAG / upstream development |
| [ocelotl-tensor](https://github.com/Opriego/ocelotl-tensor) | C++20 compiler / SSA-inspired IR / LLVM backend        |
| [linux](https://github.com/Opriego/linux)                   | Linux kernel / upstream patch development              |
| [c-systems-lab](https://github.com/Opriego/c-systems-lab)   | C17 / POSIX / concurrency / networking / memory        |
| [ecutools](https://github.com/Opriego/ecutools)             | Existing C codebase / CAN / J2534 / Linux              |
| [ocelotl_SGBDD](https://github.com/Opriego/ocelotl_SGBDD)   | Historical parser / distributed systems project        |

---

## Current Focus

I am particularly interested in work involving:

* LLVM and GCC compiler engineering
* compiler backends and instruction selection
* SelectionDAG / target legalization
* GPU and heterogeneous compilation
* CPU/GPU architecture
* performance optimization
* AI/ML compiler infrastructure
* upstream open-source development
* Linux and hardware/software interaction
* difficult cross-layer debugging

I intend to continue contributing upstream to LLVM across multiple areas, with an emphasis on correctness, code generation, legalization, optimization, and hardware-aware compiler behavior.

---

**Location:** Guadalajara, Mexico
**GitHub:** [Opriego](https://github.com/Opriego)

Open to remote and international opportunities in **compiler, GPU, systems, and open-source engineering**.
