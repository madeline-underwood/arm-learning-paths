---
title: Migrate x86 SIMD code to the Arm architecture
weight: 3

### FIXED, DO NOT MODIFY
layout: learningpathall
---

## Vectorization on x86 and Arm

Migrating SIMD (Single Instruction, Multiple Data) code from x86 extensions to Arm extensions is a key task for developers looking to optimize performance on Arm platforms. 

To ensure portability and optimal performance, it is essential to understand the mapping between x86 and Arm instruction sets. This Learning Path provides an overview to help you devise a migration plan, leveraging Arm features such as scalable vector lengths and advanced matrix operations, to effectively adapt your code.

Vectorization processes multiple data elements with one instruction. It powers HPC, AI and ML, signal processing, and data analytics. Both x86 and Arm provide rich SIMD features, but the designs differ. The x86 architecture provides fixed-width vector units of 128, 256, and 512 bits. The Arm architecture offers a mix of fixed-width (for NEON), and scalable vectors ranging from 128 to 2048 bits (for SVE and SME).

If you are migrating SIMD software to Arm, understanding these differences helps you to create portable, high-performance code.

## Arm vector and matrix extensions

### NEON

NEON is a 128-bit SIMD extension available across Armv8 cores, including mobile and Neoverse. It is well suited to multimedia, digital signal processing (DSP), and packet processing workloads. Conceptually, NEON aligns with x86 SSE or AVX at 128 bits, making it the primary target for migrating SSE workloads. Compiler support for auto-vectorization to NEON is mature, which simplifies migration.
 
### Scalable Vector Extension (SVE)

SVE introduces a revolutionary approach to SIMD with its vector-length agnostic (VLA) design. Registers in SVE can range from 128 to 2048 bits, with the exact width determined by the hardware implementation in multiples of 128 bits. This flexibility allows the same binary to run efficiently across different hardware generations. SVE also features advanced capabilities like per-element predication, which eliminates branch divergence, and native support for gather/scatter operations, enabling efficient handling of irregular memory accesses. While SVE is ideal for high-performance computing (HPC) and future-proof portability, developers must adapt to its unique programming model, which differs significantly from fixed-width SIMD paradigms. SVE is most similar to AVX-512 on x86, offering greater portability and scalability.

### Scalable Matrix Extension (SME)

SME accelerates matrix multiplication and conceptually aligns with AMX. SME uses outer-product operations, integrates with SVE, and supports scalable tiles and streaming mode. It targets AI training and inference and dense linear algebra in HPC applications.

## x86 vector and matrix extensions

### Streaming SIMD Extensions (SSE)

SSE provides 128-bit XMM registers for integer and floating-point SIMD. Despite its age, SSE remains widely used. SSE maps cleanly to Arm NEON, enabling a straightforward transition.

### Advanced Vector Extensions (AVX)

AVX introduces 256-bit YMM registers and AVX-512 adds 512-bit ZMM registers. Key features include fused multiply-add (FMA), masking in AVX-512, and VEX/EVEX encodings. Migrating AVX often means using NEON for 128-bit paths and SVE for scalable-width code. Refactor fixed-width assumptions to adopt vector-length-agnostic programming.

### Advanced Matrix Extensions (AMX)

AMX uses dedicated matrix-tile registers for GEMM and convolution-heavy AI workloads. When migrating AMX code, target Arm SME. SME’s outer-product model and streaming mode differ from AMX, so update your kernels to match SME semantics.

## Comparison tables

### SSE vs NEON

| Feature                    | SSE                                           | NEON                                                     |
|---------------------------|-----------------------------------------------|----------------------------------------------------------|
| **Register width**        | 128-bit (XMM)                                 | 128-bit (Q)                                             |
| **Vector length model**   | Fixed 128 bits                                | Fixed 128 bits                                          |
| **Predication / masking** | Minimal; no full mask registers               | Conditional select; no hardware mask registers          |
| **Gather / scatter**      | Not native in SSE (appears in AVX2+)          | Not native; use software patterns                       |
| **Instruction scope**     | Arithmetic, logical, shuffle, convert, blend  | Arithmetic, logical, shuffle, saturating ops, crypto    |
| **FP support**            | Single and double precision                   | Single and double precision                             |
| **Typical uses**          | Legacy SIMD, general vector arithmetic        | Multimedia, DSP, crypto, embedded compute               |
| **Extensibility**         | Extended by AVX families                      | Fixed at 128 bits; SVE adds scalable vectors            |
| **Programming model**     | C/C++ intrinsics, some assembly               | Intrinsics; inline assembly less common                 |

### AVX / AVX-512 vs SVE / SVE2

| Feature                    | x86: AVX / AVX-512                            | Arm: SVE / SVE2                                         |
|---------------------------|-----------------------------------------------|---------------------------------------------------------|
| **Register width**        | Fixed 256-bit (YMM), 512-bit (ZMM)            | Scalable 128–2048 bits (×128-bit increments)            |
| **Vector length model**   | Fixed; may require multiple code paths        | Vector-length agnostic; one binary scales               |
| **Predication / masking** | AVX-512 mask registers                        | Rich predication with predicate registers               |
| **Gather / scatter**      | Native in AVX2 and AVX-512                    | Native with scalable efficiency                         |
| **Key operations**        | Wide SIMD, FMA, masking, conflict detection   | Wide SIMD, FMA, predication, gather/scatter, reductions |
| **Best suited for**       | HPC, AI, scientific compute, analytics        | HPC, AI, scientific compute, cloud-scale workloads      |
| **Limitations**           | Power/thermal constraints at 512-bit widths   | Requires vector-length-agnostic style                   |

### AMX vs SME

| Feature                    | x86: AMX                                       | Arm: SME                                                |
|---------------------------|-------------------------------------------------|---------------------------------------------------------|
| **Tile model**            | Fixed-size matrix tiles                         | Scalable tiles integrated with SVE                      |
| **Vector length model**   | Fixed tile dimensions                           | Scales with SVE vector length                           |
| **Predication / masking** | No dedicated tile predication                   | Predication via SVE predicates                          |
| **Gather / scatter**      | Not within AMX tiles                            | Via SVE integration                                     |
| **Key operations**        | Dot-product GEMM and convolutions               | Outer-product GEMM with streaming mode                  |
| **Best suited for**       | AI/ML training and inference                    | AI/ML, scientific computing, dense linear algebra       |

## Key differences for developers

### Vector length model

SSE, AVX, and AVX-512 use fixed widths (128, 256, 512). This ca
