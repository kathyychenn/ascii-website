

== ./llvm/llvm_0.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/compiler-rt/test/profile/instrprof-darwin-dead-strip.c#L60-L67

```c
< ASCII >
// PRF_NAMES: Contents of (__DATA,__llvm_prf_names) section
// PRF_NAMES-NEXT: {{.*}} 08 00 66 6f 6f 01 6d 61 69 6e
//                        |  |  f  o  o  #  m  a  i  n
//                        |  |___________|
//                        |              |
//               UncompressedLen = 8     |
//                                       |
//                                CompressedLen = 0
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_1.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/compiler-rt/test/profile/instrprof-gc-sections.c#L66-L73

```c
< ASCII >
// PRF_NAMES: Hex dump of section '__llvm_prf_names':
// PRF_NAMES-NEXT: {{.*}} 0800666f 6f016d61 696e{{.*$}}
//                        | | f o  o # m a  i n
//                        | |___________|
//                        |             |
//               UncompressedLen = 8    |
//                                      |
//                               CompressedLen = 0
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_10.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libcxx/test/std/ranges/range.adaptors/range.zip/end.pass.cpp#L19-L65

```c
< ASCII >
// ID | simple | common | bidi | random | sized | #views |     v.end()    | as_const(v)
//    |        |        |      | access |       |        |                |   .end()
// ---|--------|--------|------|--------|-------|--------|----------------|---------------
// 1  |   Y    |   Y    |  Y   |    Y   |   Y   |   1    | iterator<true> | iterator<true>
// 2  |   Y    |   Y    |  Y   |    Y   |   Y   |   >1   | iterator<true> | iterator<true>
// 3  |   Y    |   N    |  Y   |    Y   |   N   |   1    | sentinel<true> | sentinel<true>
// 4  |   Y    |   N    |  Y   |    Y   |   N   |   >1   | sentinel<true> | sentinel<true>
// 5  |   Y    |   Y    |  Y   |    N   |   Y   |   1    | iterator<true> | iterator<true>
// 6  |   Y    |   Y    |  Y   |    N   |   Y   |   >1   | sentinel<true> | sentinel<true>
// 7  |   Y    |   Y    |  Y   |    N   |   N   |   1    | iterator<true> | iterator<true>
// 8  |   Y    |   Y    |  Y   |    N   |   N   |   >1   | sentinel<true> | sentinel<true>
// 9  |   Y    |   Y    |  N   |    N   |   Y   |   1    | iterator<true> | iterator<true>
// 10 |   Y    |   Y    |  N   |    N   |   Y   |   >1   | iterator<true> | iterator<true>
// 11 |   Y    |   Y    |  N   |    N   |   N   |   1    | iterator<true> | iterator<true>
// 12 |   Y    |   Y    |  N   |    N   |   N   |   >1   | iterator<true> | iterator<true>
// 13 |   Y    |   N    |  Y   |    Y   |   Y   |   1    | iterator<true> | iterator<true>
// 14 |   Y    |   N    |  Y   |    Y   |   Y   |   >1   | iterator<true> | iterator<true>
// 15 |   Y    |   N    |  Y   |    N   |   Y   |   1    | sentinel<true> | sentinel<true>
// 16 |   Y    |   N    |  Y   |    N   |   Y   |   >1   | sentinel<true> | sentinel<true>
// 17 |   Y    |   N    |  Y   |    N   |   N   |   1    | sentinel<true> | sentinel<true>
// 18 |   Y    |   N    |  Y   |    N   |   N   |   >1   | sentinel<true> | sentinel<true>
// 19 |   Y    |   N    |  N   |    N   |   Y   |   1    | sentinel<true> | sentinel<true>
// 20 |   Y    |   N    |  N   |    N   |   Y   |   >1   | sentinel<true> | sentinel<true>
// 21 |   Y    |   N    |  N   |    N   |   N   |   1    | sentinel<true> | sentinel<true>
// 22 |   Y    |   N    |  N   |    N   |   N   |   >1   | sentinel<true> | sentinel<true>
// 23 |   N    |   Y    |  Y   |    Y   |   Y   |   1    | iterator<false>| iterator<true>
// 24 |   N    |   Y    |  Y   |    Y   |   Y   |   >1   | iterator<false>| iterator<true>
// 25 |   N    |   N    |  Y   |    Y   |   N   |   1    | sentinel<false>| sentinel<true>
// 26 |   N    |   N    |  Y   |    Y   |   N   |   >1   | sentinel<false>| sentinel<true>
// 27 |   N    |   Y    |  Y   |    N   |   Y   |   1    | iterator<false>| iterator<true>
// 28 |   N    |   Y    |  Y   |    N   |   Y   |   >1   | sentinel<false>| sentinel<true>
// 29 |   N    |   Y    |  Y   |    N   |   N   |   1    | iterator<false>| iterator<true>
// 30 |   N    |   Y    |  Y   |    N   |   N   |   >1   | sentinel<false>| sentinel<true>
// 31 |   N    |   Y    |  N   |    N   |   Y   |   1    | iterator<false>| iterator<true>
// 32 |   N    |   Y    |  N   |    N   |   Y   |   >1   | iterator<false>| iterator<true>
// 33 |   N    |   Y    |  N   |    N   |   N   |   1    | iterator<false>| iterator<true>
// 34 |   N    |   Y    |  N   |    N   |   N   |   >1   | iterator<false>| iterator<true>
// 35 |   N    |   N    |  Y   |    Y   |   Y   |   1    | iterator<false>| iterator<true>
// 36 |   N    |   N    |  Y   |    Y   |   Y   |   >1   | iterator<false>| iterator<true>
// 37 |   N    |   N    |  Y   |    N   |   Y   |   1    | sentinel<false>| sentinel<true>
// 38 |   N    |   N    |  Y   |    N   |   Y   |   >1   | sentinel<false>| sentinel<true>
// 39 |   N    |   N    |  Y   |    N   |   N   |   1    | sentinel<false>| sentinel<true>
// 40 |   N    |   N    |  Y   |    N   |   N   |   >1   | sentinel<false>| sentinel<true>
// 41 |   N    |   N    |  N   |    N   |   Y   |   1    | sentinel<false>| sentinel<true>
// 42 |   N    |   N    |  N   |    N   |   Y   |   >1   | sentinel<false>| sentinel<true>
// 43 |   N    |   N    |  N   |    N   |   N   |   1    | sentinel<false>| sentinel<true>
// 44 |   N    |   N    |  N   |    N   |   N   |   >1   | sentinel<false>| sentinel<true>
< ASCII >
```
## Visual type:
- #table


== ./llvm/llvm_100.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrData.td#L259-L267

```c
< ASCII >
/// ---------------+-------------+-------------+---------
///  F  E  D  C  B | A | 9  8  7 | 6 | 5  4  3 | 2  1  0
/// ---------------+---+---------+---+---------+---------
///  0  1  0  0  1 | D | 0  0  1 | S |   MODE  |   REG
/// ---------------+---+---------+---+---------+---------
///                  REGISTER LIST MASK
/// -----------------------------------------------------
/// D - direction(RM,MR)
/// S - size(W,L)
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_101.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrData.td#L349-L354

```c
< ASCII >
/// --------------------------------------------------
///  F  E  D  C  B  A  9  8  7  6 | 5  4  3 | 2  1  0
/// --------------------------------------------------
///                               | EFFECTIVE ADDRESS
///  0  1  0  0  0  1  0  0  1  1 |   MODE  |   REG
/// --------------------------------------------------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_102.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrData.td#L378-L384

```c
< ASCII >
/// Move from CCR
/// --------------------------------------------------
///  F  E  D  C  B  A  9  8  7  6 | 5  4  3 | 2  1  0
/// --------------------------------------------------
///                               | EFFECTIVE ADDRESS
///  0  1  0  0  0  0  1  0  1  1 |   MODE  |   REG
/// --------------------------------------------------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_103.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrData.td#L422-L426

```c
< ASCII >
/// ----------------------------------------------------
///  F  E  D  C | B  A  9 | 8  7  6 | 5  4  3 | 2  1  0
/// ----------------------------------------------------
///  0  1  0  0 | DST REG | 1  1  1 |   MODE  |   REG
/// ----------------------------------------------------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_104.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrShiftRotate.td#L38-L42

```c
< ASCII >
/// ------------+---------+---+------+---+------+---------
///  F  E  D  C | B  A  9 | 8 | 7  6 | 5 | 4  3 | 2  1  0
/// ------------+---------+---+------+---+------+---------
///  1  1  1  0 | REG/IMM | D | SIZE |R/I|  OP  |   REG
/// ------------+---------+---+------+---+------+---------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_105.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/PowerPC/PPCISelLowering.cpp#L6883-L6921

```c
//   AIX ABI Stack Frame Layout:
//
< ASCII >
//   Low Memory +--------------------------------------------+
//   SP   +---> | Back chain                                 | ---+
//        |     +--------------------------------------------+    |   
//        |     | Saved Condition Register                   |    |
//        |     +--------------------------------------------+    |
//        |     | Saved Linkage Register                     |    |
//        |     +--------------------------------------------+    | Linkage Area
//        |     | Reserved for compilers                     |    |
//        |     +--------------------------------------------+    |
//        |     | Reserved for binders                       |    |
//        |     +--------------------------------------------+    |
//        |     | Saved TOC pointer                          | ---+
//        |     +--------------------------------------------+
//        |     | Parameter save area                        |
//        |     +--------------------------------------------+
//        |     | Alloca space                               |
//        |     +--------------------------------------------+
//        |     | Local variable space                       |
//        |     +--------------------------------------------+
//        |     | Float/int conversion temporary             |
//        |     +--------------------------------------------+
//        |     | Save area for AltiVec registers            |
//        |     +--------------------------------------------+
//        |     | AltiVec alignment padding                  |
//        |     +--------------------------------------------+
//        |     | Save area for VRSAVE register              |
//        |     +--------------------------------------------+
//        |     | Save area for General Purpose registers    |
//        |     +--------------------------------------------+
//        |     | Save area for Floating Point registers     |
//        |     +--------------------------------------------+
//        +---- | Back chain                                 |
// High Memory  +--------------------------------------------+
< ASCII >
//
//  Specifications:
//  AIX 7.2 Assembler Language Reference
//  Subroutine linkage convention
```
## Visual type:
- #memory-layout


== ./llvm/llvm_106.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/RISCV/RISCVMergeBaseOffset.cpp#L152-L171

```c
// Detect patterns for large offsets that are passed into an ADD instruction.
// If the pattern is found, updates the offset in Hi and Lo instructions
// and deletes TailAdd and the instructions that produced the offset.
//
< ASCII >
//                     Base address lowering is of the form:
//                       Hi:  lui   vreg1, %hi(s)
//                       Lo:  addi  vreg2, vreg1, %lo(s)
//                       /                                  \
//                      /                                    \
//                     /                                      \
//                    /  The large offset can be of two forms: \
//  1) Offset that has non zero bits in lower      2) Offset that has non zero
//     12 bits and upper 20 bits                      bits in upper 20 bits only
//   OffseLUI: lui   vreg3, 4
// OffsetTail: addi  voff, vreg3, 188                OffsetTail: lui  voff, 128
//                    \                                        /
//                     \                                      /
//                      \                                    /
//                       \                                  /
//                         TailAdd: add  vreg4, vreg2, voff
< ASCII >
```
## Visual type:
- #program-graph


== ./llvm/llvm_107.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/WebAssembly/WebAssemblyExceptionInfo.h#L25-L40

```c
< ASCII >
// WebAssembly instructions for exception handling are structured as follows:
//   try
//     instructions*
//   catch             ----|
//     instructions*       | -> A WebAssemblyException consists of this region
//   end               ----|
< ASCII >
//
// A WebAssemblyException object contains BBs that belong to a 'catch' part of
// the try-catch-end structure to be created later. 'try' and 'end' markers
// are not present at this stage and will be generated in CFGStackify pass.
// Because CFGSort requires all the BBs within a catch part to be sorted
// together as it does for loops, this pass calculates the nesting structure of
// catch part of exceptions in a function.
//
// An exception catch part is defined as a BB with catch instruction and all
// other BBs dominated by this BB.
```
## Visual type:
- #annotation


== ./llvm/llvm_108.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/X86/X86ExpandPseudo.cpp#L609-L622

```c
// This function creates additional block for storing varargs guarded
// registers. It adds check for %al into entry block, to skip
// GuardedRegsBlk if xmm registers should not be stored.
//
< ASCII >
//     EntryBlk[VAStartPseudoInstr]     EntryBlk
//        |                              |     .
//        |                              |        .
//        |                              |   GuardedRegsBlk
//        |                      =>      |        .
//        |                              |     .
//        |                             TailBlk
//        |                              |
//        |                              |
< ASCII >
//
```
## Visual type:
- #custom


== ./llvm/llvm_109.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/X86/X86LowerAMXType.cpp#L1-L40

```c
//===- Target/X86/X86LowerAMXType.cpp - -------------------------*- C++ -*-===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
/// \file Pass to transform <256 x i32> load/store
/// <256 x i32> is bitcasted to x86_amx on X86, and AMX instruction set only
/// provides simple operation on x86_amx. The basic elementwise operation
/// is not supported by AMX. Since x86_amx is bitcasted from vector <256 x i32>
/// and only AMX intrinsics can operate on the type, we need transform
/// load/store <256 x i32> instruction to AMX load/store. If the bitcast can
/// not be combined with load/store, we transform the bitcast to amx load/store
/// and <256 x i32> store/load.
///
/// If Front End not use O0 but the Mid/Back end use O0, (e.g. "Clang -O2 -S
/// -emit-llvm t.c" + "llc t.ll") we should make sure the amx data is volatile,
/// because that is necessary for AMX fast register allocation. (In Fast
/// registera allocation, register will be allocated before spill/reload, so
/// there is no additional register for amx to identify the step in spill.)
/// The volatileTileData() will handle this case.
< ASCII >
/// e.g.
/// ----------------------------------------------------------
/// | def %td = ...                                          |
/// | ...                                                    |
/// | "use %td"                                              |
/// ----------------------------------------------------------
/// will transfer to -->
/// ----------------------------------------------------------
/// | def %td = ...                                          |
/// | call void @llvm.x86.tilestored64.internal(mem, %td)    |
/// | ...                                                    |
/// | %td2 = call x86_amx @llvm.x86.tileloadd64.internal(mem)|
/// | "use %td2"                                             |
/// ----------------------------------------------------------
< ASCII >
//
//===----------------------------------------------------------------------===//
//
```
## Visual type:
- #custom


== ./llvm/llvm_11.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libcxx/test/std/time/time.hms/time.hms.members/hours.pass.cpp#L16-L24

```c
< ASCII >
// Test values
// duration     hours   minutes seconds fractional
// 5000s            1       23      20      0
// 5000 minutes     83      20      0       0
// 123456789ms      34      17      36      789ms
// 123456789us      0       2       3       456789us
// 123456789ns      0       0       0       123456789ns
// 1000mfn          0       20      9       0.6 (6000/10000)
// 10000mfn         3       21      36      0
< ASCII >
```
## Visual type:
- #table


== ./llvm/llvm_110.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Scalar/DFAJumpThreading.cpp#L1-L58

```c
//===- DFAJumpThreading.cpp - Threads a switch statement inside a loop ----===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// Transform each threading path to effectively jump thread the DFA. For
// example, the CFG below could be transformed as follows, where the cloned
// blocks unconditionally branch to the next correct case based on what is
// identified in the analysis.
//
< ASCII >
//          sw.bb                        sw.bb
//        /   |   \                    /   |   \
//   case1  case2  case3          case1  case2  case3
//        \   |   /                 |      |      |
//       determinator            det.2   det.3  det.1
//        br sw.bb                /        |        \
//                          sw.bb.2     sw.bb.3     sw.bb.1
//                           br case2    br case3    br case1§
< ASCII >
//
// Definitions and Terminology:
//
// * Threading path:
//   a list of basic blocks, the exit state, and the block that determines
//   the next state, for which the following notation will be used:
//   < path of BBs that form a cycle > [ state, determinator ]
//
// * Predictable switch:
//   The switch variable is always a known constant so that all conditional
//   jumps based on switch variable can be converted to unconditional jump.
//
// * Determinator:
//   The basic block that determines the next state of the DFA.
//
// Representing the optimization in C-like pseudocode: the code pattern on the
// left could functionally be transformed to the right pattern if the switch
// condition is predictable.
//
< ASCII >
//  X = A                       goto A
//  for (...)                   A:
//    switch (X)                  ...
//      case A                    goto B
//        X = B                 B:
//      case B                    ...
//        X = C                   goto C
< ASCII >
//
// The pass first checks that switch variable X is decided by the control flow
// path taken in the loop; for example, in case B, the next value of X is
// decided to be C. It then enumerates through all paths in the loop and labels
// the basic blocks where the next state is decided.
//
// Using this information it creates new paths that unconditionally branch to
// the next case. This involves cloning code, so it only gets triggered if the
// amount of code duplicated is below a threshold.
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_111.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Scalar/GVNHoist.cpp#L131-L140

```c
// CHI keeps information about values flowing out of a basic block.  It is
// similar to PHI but in the inverse graph, and used for outgoing values on each
// edge. For conciseness, it is computed only for instructions with multiple
// occurrences in the CFG because they are the only hoistable candidates.
< ASCII >
//     A (CHI[{V, B, I1}, {V, C, I2}]
//  /     \
// /       \
// B(I1)  C (I2)
< ASCII >
// The Value number for both I1 and I2 is V, the CHI node will save the
// instruction as well as the edge where the value is flowing to.
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_112.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Scalar/LoopVersioningLICM.cpp#L1-L60

```c
//===- LoopVersioningLICM.cpp - LICM Loop Versioning ----------------------===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// When alias analysis is uncertain about the aliasing between any two accesses,
// it will return MayAlias. This uncertainty from alias analysis restricts LICM
// from proceeding further. In cases where alias analysis is uncertain we might
// use loop versioning as an alternative.
//
// Loop Versioning will create a version of the loop with aggressive aliasing
// assumptions in addition to the original with conservative (default) aliasing
// assumptions. The version of the loop making aggressive aliasing assumptions
// will have all the memory accesses marked as no-alias. These two versions of
// loop will be preceded by a memory runtime check. This runtime check consists
// of bound checks for all unique memory accessed in loop, and it ensures the
// lack of memory aliasing. The result of the runtime check determines which of
// the loop versions is executed: If the runtime check detects any memory
// aliasing, then the original loop is executed. Otherwise, the version with
// aggressive aliasing assumptions is used.
//
// Following are the top level steps:
//
// a) Perform LoopVersioningLICM's feasibility check.
// b) If loop is a candidate for versioning then create a memory bound check,
//    by considering all the memory accesses in loop body.
// c) Clone original loop and set all memory accesses as no-alias in new loop.
// d) Set original loop & versioned loop as a branch target of the runtime check
//    result.
//
< ASCII >
// It transforms loop as shown below:
//
//                         +----------------+
//                         |Runtime Memcheck|
//                         +----------------+
//                                 |
//              +----------+----------------+----------+
//              |                                      |
//    +---------+----------+               +-----------+----------+
//    |Orig Loop Preheader |               |Cloned Loop Preheader |
//    +--------------------+               +----------------------+
//              |                                      |
//    +--------------------+               +----------------------+
//    |Orig Loop Body      |               |Cloned Loop Body      |
//    +--------------------+               +----------------------+
//              |                                      |
//    +--------------------+               +----------------------+
//    |Orig Loop Exit Block|               |Cloned Loop Exit Block|
//    +--------------------+               +-----------+----------+
//              |                                      |
//              +----------+--------------+-----------+
//                                 |
//                           +-----+----+
//                           |Join Block|
//                           +----------+
< ASCII >
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #program-graph


== ./llvm/llvm_113.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Scalar/LowerMatrixIntrinsics.cpp#L131-L147

```c
// Given an element pointer \p BasePtr to the start of a (sub) matrix, compute
// the start address of vector \p VecIdx with type (\p EltType x \p NumElements)
// assuming \p Stride elements between start two consecutive vectors.
// \p Stride must be >= \p NumElements.
// For column-major matrixes, the function computes the address of a column
// vectors and \p NumElements must be set to the number of elements in a column
// (= number of rows of the matrix). For row-major matrixes, the function
// computes the address of a row vector and \p NumElements must be set to the
// number of elements in a column (= number of columns of the matrix).
//
// Consider a 4x4 matrix in column-mjaor layout like below
//
< ASCII >
//      0       1      2      3
// 0   v_0_0  v_0_1  v_0_2  v_0_3
// 1   v_1_0  v_1_1  v_1_2  v_1_3
// 2   v_2_0  v_2_1  v_2_2  v_2_3
// 3   v_3_0  v_3_1  v_3_2  v_3_3
< ASCII >
```
## Visual type:
- #matrix


== ./llvm/llvm_114.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Scalar/LowerMatrixIntrinsics.cpp#L149-L170

```c
// To compute the column addresses for a 2x3 sub-matrix at row 1 and column 1,
// we need a pointer to the first element of the submatrix as base pointer.
// Then we can use computeVectorAddr to compute the addresses for the columns
// of the sub-matrix.
//
// Column 0: computeVectorAddr(Base, 0 (column), 4 (stride), 2 (num rows), ..)
//           -> just returns Base
// Column 1: computeVectorAddr(Base, 1 (column), 4 (stride), 2 (num rows), ..)
//           -> returns Base + (1 * 4)
// Column 2: computeVectorAddr(Base, 2 (column), 4 (stride), 2 (num rows), ..)
//           -> returns Base + (2 * 4)
//
// The graphic below illustrates the number of elements in a column (marked
// with |) and the number of skipped elements (marked with }).
//
< ASCII >
//         v_0_0  v_0_1 {v_0_2 {v_0_3
//                Base   Col 1  Col 2
//                  |     |      |
//         v_1_0 |v_1_1 |v_1_2 |v_1_3
//         v_2_0 |v_2_1 |v_2_2 |v_2_3
//         v_3_0 {v_3_1 {v_3_2  v_3_3
< ASCII >
//
```
## Visual type:
- #matrix


== ./llvm/llvm_115.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Scalar/MergedLoadStoreMotion.cpp#L1-L76

```c
//===- MergedLoadStoreMotion.cpp - merge and hoist/sink load/stores -------===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
//! \file
//! This pass performs merges of loads and stores on both sides of a
//  diamond (hammock). It hoists the loads and sinks the stores.
//
// The algorithm iteratively hoists two loads to the same address out of a
// diamond (hammock) and merges them into a single load in the header. Similar
// it sinks and merges two stores to the tail block (footer). The algorithm
// iterates over the instructions of one side of the diamond and attempts to
// find a matching load/store on the other side. New tail/footer block may be
// insterted if the tail/footer block has more predecessors (not only the two
// predecessors that are forming the diamond). It hoists / sinks when it thinks
// it safe to do so.  This optimization helps with eg. hiding load latencies,
// triggering if-conversion, and reducing static code size.
//
// NOTE: This code no longer performs load hoisting, it is subsumed by GVNHoist.
//
//===----------------------------------------------------------------------===//
//
//
< ASCII >
// Example:
// Diamond shaped code before merge:
//
//            header:
//                     br %cond, label %if.then, label %if.else
//                        +                    +
//                       +                      +
//                      +                        +
//            if.then:                         if.else:
//               %lt = load %addr_l               %le = load %addr_l
//               <use %lt>                        <use %le>
//               <...>                            <...>
//               store %st, %addr_s               store %se, %addr_s
//               br label %if.end                 br label %if.end
//                     +                         +
//                      +                       +
//                       +                     +
//            if.end ("footer"):
//                     <...>
//
// Diamond shaped code after merge:
//
//            header:
//                     %l = load %addr_l
//                     br %cond, label %if.then, label %if.else
//                        +                    +
//                       +                      +
//                      +                        +
//            if.then:                         if.else:
//               <use %l>                         <use %l>
//               <...>                            <...>
//               br label %if.end                 br label %if.end
//                      +                        +
//                       +                      +
//                        +                    +
//            if.end ("footer"):
//                     %s.sink = phi [%st, if.then], [%se, if.else]
//                     <...>
//                     store %s.sink, %addr_s
//                     <...>
< ASCII >
//
//
//===----------------------- TODO -----------------------------------------===//
//
// 1) Generalize to regions other than diamonds
// 2) Be more aggressive merging memory operations
// Note that both changes require register pressure control
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #custom


== ./llvm/llvm_116.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Scalar/Reassociate.cpp#L387-L459

```c
/// Given an associative binary expression, return the leaf
/// nodes in Ops along with their weights (how many times the leaf occurs).  The
/// original expression is the same as
///   (Ops[0].first op Ops[0].first op ... Ops[0].first)  <- Ops[0].second times
/// op
///   (Ops[1].first op Ops[1].first op ... Ops[1].first)  <- Ops[1].second times
/// op
///   ...
/// op
///   (Ops[N].first op Ops[N].first op ... Ops[N].first)  <- Ops[N].second times
///
/// Note that the values Ops[0].first, ..., Ops[N].first are all distinct.
///
/// This routine may modify the function, in which case it returns 'true'.  The
/// changes it makes may well be destructive, changing the value computed by 'I'
/// to something completely different.  Thus if the routine returns 'true' then
/// you MUST either replace I with a new expression computed from the Ops array,
/// or use RewriteExprTree to put the values back in.
///
/// A leaf node is either not a binary operation of the same kind as the root
/// node 'I' (i.e. is not a binary operator at all, or is, but with a different
/// opcode), or is the same kind of binary operator but has a use which either
/// does not belong to the expression, or does belong to the expression but is
/// a leaf node.  Every leaf node has at least one use that is a non-leaf node
/// of the expression, while for non-leaf nodes (except for the root 'I') every
/// use is a non-leaf node of the expression.
///
< ASCII >
/// For example:
///           expression graph        node names
///
///                     +        |        I
///                    / \       |
///                   +   +      |      A,  B
///                  / \ / \     |
///                 *   +   *    |    C,  D,  E
///                / \ / \ / \   |
///                   +   *      |      F,  G
< ASCII >
///
/// The leaf nodes are C, E, F and G.  The Ops array will contain (maybe not in
/// that order) (C, 1), (E, 1), (F, 2), (G, 2).
///
/// The expression is maximal: if some instruction is a binary operator of the
/// same kind as 'I', and all of its uses are non-leaf nodes of the expression,
/// then the instruction also belongs to the expression, is not a leaf node of
/// it, and its operands also belong to the expression (but may be leaf nodes).
///
/// NOTE: This routine will set operands of non-leaf non-root nodes to undef in
/// order to ensure that every non-root node in the expression has *exactly one*
/// use by a non-leaf node of the expression.  This destruction means that the
/// caller MUST either replace 'I' with a new expression or use something like
/// RewriteExprTree to put the values back in if the routine indicates that it
/// made a change by returning 'true'.
///
/// In the above example either the right operand of A or the left operand of B
/// will be replaced by undef.  If it is B's operand then this gives:
///
< ASCII >
///                     +        |        I
///                    / \       |
///                   +   +      |      A,  B - operand of B replaced with undef
///                  / \   \     |
///                 *   +   *    |    C,  D,  E
///                / \ / \ / \   |
///                   +   *      |      F,  G
< ASCII >
///
/// Note that such undef operands can only be reached by passing through 'I'.
/// For example, if you visit operands recursively starting from a leaf node
/// then you will never see such an undef operand unless you get back to 'I',
/// which requires passing through a phi node.
///
/// Note that this routine may also mutate binary operators of the wrong type
/// that have all uses inside the expression (i.e. only used by non-leaf nodes
/// of the expression) if it can turn them into binary operators of the right
/// type and thus make the expression bigger.
```
## Visual type:
- #custom


== ./llvm/llvm_117.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Utils/FixIrreducible.cpp#L1-L67

```c
//===- FixIrreducible.cpp - Convert irreducible control-flow into loops ---===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// An irreducible SCC is one which has multiple "header" blocks, i.e., blocks
// with control-flow edges incident from outside the SCC.  This pass converts a
// irreducible SCC into a natural loop by applying the following transformation:
//
// 1. Collect the set of headers H of the SCC.
// 2. Collect the set of predecessors P of these headers. These may be inside as
//    well as outside the SCC.
// 3. Create block N and redirect every edge from set P to set H through N.
//
// This converts the SCC into a natural loop with N as the header: N is the only
// block with edges incident from outside the SCC, and all backedges in the SCC
// are incident on N, i.e., for every backedge, the head now dominates the tail.
//
< ASCII >
// INPUT CFG: The blocks A and B form an irreducible loop with two headers.
//
//                        Entry
//                       /     \
//                      v       v
//                      A ----> B
//                      ^      /|
//                       `----' |
//                              v
//                             Exit
//
// OUTPUT CFG: Edges incident on A and B are now redirected through a
// new block N, forming a natural loop consisting of N, A and B.
//
//                        Entry
//                          |
//                          v
//                    .---> N <---.
//                   /     / \     \
//                  |     /   \     |
//                  \    v     v    /
//                   `-- A     B --'
//                             |
//                             v
//                            Exit
< ASCII >
//
// The transformation is applied to every maximal SCC that is not already
// recognized as a loop. The pass operates on all maximal SCCs found in the
// function body outside of any loop, as well as those found inside each loop,
// including inside any newly created loops. This ensures that any SCC hidden
// inside a maximal SCC is also transformed.
//
// The actual transformation is handled by function CreateControlFlowHub, which
// takes a set of incoming blocks (the predecessors) and outgoing blocks (the
// headers). The function also moves every PHINode in an outgoing block to the
// hub. Since the hub dominates all the outgoing blocks, each such PHINode
// continues to dominate its uses. Since every header in an SCC has at least two
// predecessors, every value used in the header (or later) but defined in a
// predecessor (or earlier) is represented by a PHINode in a header. Hence the
// above handling of PHINodes is sufficient and no further processing is
// required to restore SSA.
//
// Limitation: The pass cannot handle switch statements and indirect
//             branches. Both must be lowered to plain branches first.
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_118.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Utils/FlattenCFG.cpp#L63-L135

```c
/// If \param [in] BB has more than one predecessor that is a conditional
/// branch, attempt to use parallel and/or for the branch condition. \returns
/// true on success.
///
< ASCII >
/// Before:
///   ......
///   %cmp10 = fcmp une float %tmp1, %tmp2
///   br i1 %cmp10, label %if.then, label %lor.rhs
///
/// lor.rhs:
///   ......
///   %cmp11 = fcmp une float %tmp3, %tmp4
///   br i1 %cmp11, label %if.then, label %ifend
///
/// if.end:  // the merge block
///   ......
///
/// if.then: // has two predecessors, both of them contains conditional branch.
///   ......
///   br label %if.end;
///
/// After:
///  ......
///  %cmp10 = fcmp une float %tmp1, %tmp2
///  ......
///  %cmp11 = fcmp une float %tmp3, %tmp4
///  %cmp12 = or i1 %cmp10, %cmp11    // parallel-or mode.
///  br i1 %cmp12, label %if.then, label %ifend
///
///  if.end:
///    ......
///
///  if.then:
///    ......
///    br label %if.end;
< ASCII >
///
///  Current implementation handles two cases.
< ASCII >
///  Case 1: BB is on the else-path.
///
///          BB1
///        /     |
///       BB2    |
///      /   \   |
///     BB3   \  |     where, BB1, BB2 contain conditional branches.
///      \    |  /     BB3 contains unconditional branch.
///       \   | /      BB4 corresponds to BB which is also the merge.
///  BB => BB4
///
///
///  Corresponding source code:
///
///  if (a == b && c == d)
///    statement; // BB3
///
///  Case 2: BB is on the then-path.
///
///             BB1
///          /      |
///         |      BB2
///         \    /    |  where BB1, BB2 contain conditional branches.
///  BB =>   BB3      |  BB3 contains unconditiona branch and corresponds
///           \     /    to BB.  BB4 is the merge.
///             BB4
///
///  Corresponding source code:
///
///  if (a == b || c == d)
///    statement;  // BB3
< ASCII >
///
///  In both cases, BB is the common successor of conditional branches.
///  In Case 1, BB (BB4) has an unconditional branch (BB3) as
///  its predecessor.  In Case 2, BB (BB3) only has conditional branches
///  as its predecessors.
```
## Visual type:
- #program-graph


== ./llvm/llvm_119.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Utils/SimplifyCFG.cpp#L2599-L2618

```c
/// If this block is a `landingpad` exception handling block, categorize all
/// the predecessor `invoke`s into sets, with all `invoke`s in each set
/// being "mergeable" together, and then merge invokes in each set together.
///
< ASCII >
/// This is a weird mix of hoisting and sinking. Visually, it goes from:
///          [...]        [...]
///            |            |
///        [invoke0]    [invoke1]
///           / \          / \
///     [cont0] [landingpad] [cont1]
/// to:
///      [...] [...]
///          \ /
///       [invoke]
///          / \
///     [cont] [landingpad]
< ASCII >
///
/// But of course we can only do that if the invokes share the `landingpad`,
/// edges invoke0->cont0 and invoke1->cont1 are "compatible",
/// and the invoked functions are "compatible".
```
## Visual type:
- #program-graph


== ./llvm/llvm_12.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libcxx/test/support/filesystem_test_helper.h#L347-L364

```c
/// This class generates the following tree:
///
< ASCII >
///     static_test_env
///     |-- bad_symlink -> dne
///     |-- dir1
///     |   |-- dir2
///     |   |   |-- afile3
///     |   |   |-- dir3
///     |   |   |   `-- file5
///     |   |   |-- file4
///     |   |   `-- symlink_to_dir3 -> dir3
///     |   `-- file1
///     |   `-- file2
///     |-- empty_file
///     |-- non_empty_file
///     |-- symlink_to_dir -> dir1
///     `-- symlink_to_empty_file -> empty_file
< ASCII >
///
```
## Visual type:
- #tree


== ./llvm/llvm_120.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Transforms/Vectorize/SLPVectorizer.cpp#L11820-L11845

```c
/// Model horizontal reductions.
///
/// A horizontal reduction is a tree of reduction instructions that has values
/// that can be put into a vector as its leaves. For example:
///
< ASCII >
/// mul mul mul mul
///  \  /    \  /
///   +       +
///    \     /
///       +
/// This tree has "mul" as its leaf values and "+" as its reduction
/// instructions. A reduction can feed into a store or a binary operation
/// feeding a phi.
///    ...
///    \  /
///     +
///     |
///  phi +=
///
///  Or:
///    ...
///    \  /
///     +
///     |
///   *p =
< ASCII >
///
```
## Visual type:
- #program-graph


== ./llvm/llvm_121.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/test/MC/MachO/variable-exprs.s#L49-L253

```c
< ASCII >
// CHECK-I386: File: <stdin>
// CHECK-I386: Format: Mach-O 32-bit i386
// CHECK-I386: Arch: i386
// CHECK-I386: AddressSize: 32bit
// CHECK-I386: MachHeader {
// CHECK-I386:   Magic: Magic (0xFEEDFACE)
// CHECK-I386:   CpuType: X86 (0x7)
// CHECK-I386:   CpuSubType: CPU_SUBTYPE_I386_ALL (0x3)
// CHECK-I386:   FileType: Relocatable (0x1)
// CHECK-I386:   NumOfLoadCommands: 4
// CHECK-I386:   SizeOfLoadCommands: 312
// CHECK-I386:   Flags [ (0x0)
// CHECK-I386:   ]
// CHECK-I386: }
// CHECK-I386: Sections [
// CHECK-I386:   Section {
// CHECK-I386:     Index: 0
// CHECK-I386:     Name: __text (5F 5F 74 65 78 74 00 00 00 00 00 00 00 00 00 00)
// CHECK-I386:     Segment: __TEXT (5F 5F 54 45 58 54 00 00 00 00 00 00 00 00 00 00)
// CHECK-I386:     Address: 0x0
// CHECK-I386:     Size: 0x1
// CHECK-I386:     Offset: 340
// CHECK-I386:     Alignment: 0
// CHECK-I386:     RelocationOffset: 0x0
// CHECK-I386:     RelocationCount: 0
// CHECK-I386:     Type: Regular (0x0)
// CHECK-I386:     Attributes [ (0x800004)
// CHECK-I386:       PureInstructions (0x800000)
// CHECK-I386:       SomeInstructions (0x4)
// CHECK-I386:     ]
// CHECK-I386:     Reserved1: 0x0
// CHECK-I386:     Reserved2: 0x0
// CHECK-I386:     SectionData (
// CHECK-I386:       0000: C3                                   |.|
// CHECK-I386:     )
// CHECK-I386:   }
// CHECK-I386:   Section {
// CHECK-I386:     Index: 1
// CHECK-I386:     Name: __data (5F 5F 64 61 74 61 00 00 00 00 00 00 00 00 00 00)
// CHECK-I386:     Segment: __DATA (5F 5F 44 41 54 41 00 00 00 00 00 00 00 00 00 00)
// CHECK-I386:     Address: 0x1
// CHECK-I386:     Size: 0x38
// CHECK-I386:     Offset: 341
// CHECK-I386:     Alignment: 0
// CHECK-I386:     RelocationOffset: 0x190
// CHECK-I386:     RelocationCount: 9
// CHECK-I386:     Type: Regular (0x0)
// CHECK-I386:     Attributes [ (0x0)
// CHECK-I386:     ]
// CHECK-I386:     Reserved1: 0x0
// CHECK-I386:     Reserved2: 0x0
// CHECK-I386:     SectionData (
// CHECK-I386:       0000: 00000000 00000000 05000000 00000000  |................|
// CHECK-I386:       0010: 05000000 09000000 09000000 00000000  |................|
// CHECK-I386:       0020: 00000000 00000000 0D000000 0D000000  |................|
// CHECK-I386:       0030: CFFFFFFF FFFFFFFF                    |........|
// CHECK-I386:     )
// CHECK-I386:   }
// CHECK-I386: ]
// CHECK-I386: Relocations [
// CHECK-I386:   Section __data {
// CHECK-I386:     0x2C 0 2 0 GENERIC_RELOC_VANILLA 0 __data
// CHECK-I386:     0x28 0 2 0 GENERIC_RELOC_VANILLA 0 __data
// CHECK-I386:     0x24 0 2 1 GENERIC_RELOC_VANILLA 0 d3
// CHECK-I386:     0x20 0 2 1 GENERIC_RELOC_VANILLA 0 d{{$}}
// CHECK-I386:     0x1C 0 2 1 GENERIC_RELOC_VANILLA 0 d{{$}}
// CHECK-I386:     0x18 0 2 n/a GENERIC_RELOC_VANILLA 1 0x5
// CHECK-I386:     0x14 0 2 0 GENERIC_RELOC_VANILLA 0 __data
// CHECK-I386:     0x10 0 2 0 GENERIC_RELOC_VANILLA 0 __data
// CHECK-I386:     0x8 0 2 0 GENERIC_RELOC_VANILLA 0 __data
// CHECK-I386:   }
// CHECK-I386: ]
// CHECK-I386: Symbols [
// CHECK-I386:   Symbol {
// CHECK-I386:     Name: a (13)
// CHECK-I386:     Type: Section (0xE)
// CHECK-I386:     Section: __data (0x2)
// CHECK-I386:     RefType: UndefinedNonLazy (0x0)
// CHECK-I386:     Flags [ (0x0)
// CHECK-I386:     ]
// CHECK-I386:     Value: 0x5
// CHECK-I386:   }
// CHECK-I386:   Symbol {
// CHECK-I386:     Name: b (11)
// CHECK-I386:     Type: Section (0xE)
// CHECK-I386:     Section: __data (0x2)
// CHECK-I386:     RefType: UndefinedNonLazy (0x0)
// CHECK-I386:     Flags [ (0x0)
// CHECK-I386:     ]
// CHECK-I386:     Value: 0x5
// CHECK-I386:   }
// CHECK-I386:   Symbol {
// CHECK-I386:     Name: c (9)
// CHECK-I386:     Type: Section (0xE)
// CHECK-I386:     Section: __data (0x2)
// CHECK-I386:     RefType: UndefinedNonLazy (0x0)
// CHECK-I386:     Flags [ (0x0)
// CHECK-I386:     ]
// CHECK-I386:     Value: 0x9
// CHECK-I386:   }
// CHECK-I386:   Symbol {
// CHECK-I386:     Name: e (5)
// CHECK-I386:     Type: Section (0xE)
// CHECK-I386:     Section: __data (0x2)
// CHECK-I386:     RefType: UndefinedNonLazy (0x0)
// CHECK-I386:     Flags [ (0x200)
// CHECK-I386:     ]
// CHECK-I386:     Value: 0x9
// CHECK-I386:   }
// CHECK-I386:   Symbol {
// CHECK-I386:     Name: g (1)
// CHECK-I386:     Type: Section (0xE)
// CHECK-I386:     Section: __data (0x2)
// CHECK-I386:     RefType: UndefinedNonLazy (0x0)
// CHECK-I386:     Flags [ (0x0)
// CHECK-I386:     ]
// CHECK-I386:     Value: 0xD
// CHECK-I386:   }
// CHECK-I386:   Symbol {
// CHECK-I386:     Name: f (3)
// CHECK-I386:     Type: Section (0xE)
// CHECK-I386:     Section: __data (0x2)
// CHECK-I386:     RefType: UndefinedNonLazy (0x0)
// CHECK-I386:     Flags [ (0x0)
// CHECK-I386:     ]
// CHECK-I386:     Value: 0xD
// CHECK-I386:   }
// CHECK-I386:   Symbol {
// CHECK-I386:     Name: t0 (21)
// CHECK-I386:     Type: Section (0xE)
// CHECK-I386:     Section: __text (0x1)
// CHECK-I386:     RefType: UndefinedNonLazy (0x0)
// CHECK-I386:     Flags [ (0x0)
// CHECK-I386:     ]
// CHECK-I386:     Value: 0x0
// CHECK-I386:   }
// CHECK-I386:   Symbol {
// CHECK-I386:     Name: d (7)
// CHECK-I386:     Extern
// CHECK-I386:     Type: Undef (0x0)
// CHECK-I386:     Section:  (0x0)
// CHECK-I386:     RefType: UndefinedNonLazy (0x0)
// CHECK-I386:     Flags [ (0x0)
// CHECK-I386:     ]
// CHECK-I386:     Value: 0x0
// CHECK-I386:   }
// CHECK-I386:   Symbol {
// CHECK-I386:     Name: d2 (18)
// CHECK-I386:     Extern
// CHECK-I386:     Type: Indirect (0xA)
// CHECK-I386:     Section:  (0x0)
// CHECK-I386:     RefType: UndefinedNonLazy (0x0)
// CHECK-I386:     Flags [ (0x0)
// CHECK-I386:     ]
// CHECK-I386:     Value: 0x7
// CHECK-I386:   }
// CHECK-I386:   Symbol {
// CHECK-I386:     Name: d3 (15)
// CHECK-I386:     Extern
// CHECK-I386:     Type: Undef (0x0)
// CHECK-I386:     Section:  (0x0)
// CHECK-I386:     RefType: UndefinedNonLazy (0x0)
// CHECK-I386:     Flags [ (0x200)
// CHECK-I386:     ]
// CHECK-I386:     Value: 0x0
// CHECK-I386:   }
// CHECK-I386: ]
// CHECK-I386: Indirect Symbols {
// CHECK-I386:   Number: 0
// CHECK-I386:   Symbols [
// CHECK-I386:   ]
// CHECK-I386: }
// CHECK-I386: Segment {
// CHECK-I386:   Cmd: LC_SEGMENT
// CHECK-I386:   Name: 
// CHECK-I386:   Size: 192
// CHECK-I386:   vmaddr: 0x0
// CHECK-I386:   vmsize: 0x39
// CHECK-I386:   fileoff: 340
// CHECK-I386:   filesize: 57
// CHECK-I386:   maxprot: rwx
// CHECK-I386:   initprot: rwx
// CHECK-I386:   nsects: 2
// CHECK-I386:   flags: 0x0
// CHECK-I386: }
// CHECK-I386: Dysymtab {
// CHECK-I386:   ilocalsym: 0
// CHECK-I386:   nlocalsym: 7
// CHECK-I386:   iextdefsym: 7
// CHECK-I386:   nextdefsym: 0
// CHECK-I386:   iundefsym: 7
// CHECK-I386:   nundefsym: 3
// CHECK-I386:   tocoff: 0
// CHECK-I386:   ntoc: 0
// CHECK-I386:   modtaboff: 0
// CHECK-I386:   nmodtab: 0
// CHECK-I386:   extrefsymoff: 0
// CHECK-I386:   nextrefsyms: 0
// CHECK-I386:   indirectsymoff: 0
// CHECK-I386:   nindirectsyms: 0
// CHECK-I386:   extreloff: 0
// CHECK-I386:   nextrel: 0
// CHECK-I386:   locreloff: 0
// CHECK-I386:   nlocrel: 0
// CHECK-I386: }
< ASCII >
```
## Visual type:
- #custom


== ./llvm/llvm_122.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/test/Object/X86/nm-print-size.s#L10-L13

```c
< ASCII >
// SYSV: a                   |0000000000000000|   n  |            NOTYPE|ffffffffffffffff|     |foo
// SYSV: b                   |0000000000000000|   N  |            NOTYPE|0000000000000000|     |foo
// SYSV: c                   |0000000000000004|   C  |            OBJECT|0000000000000004|     |*COM*
// SYSV: d                   |ffffffffffffffff|   a  |            NOTYPE|0000000000000000|     |*ABS*
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_123.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/tools/llvm-exegesis/lib/SchedClassResolution.cpp#L22-L49

```c
// Return the non-redundant list of WriteProcRes used by the given sched class.
// The scheduling model for LLVM is such that each instruction has a certain
// number of uops which consume resources which are described by WriteProcRes
// entries. Each entry describe how many cycles are spent on a specific ProcRes
// kind.
// For example, an instruction might have 3 uOps, one dispatching on P0
// (ProcResIdx=1) and two on P06 (ProcResIdx = 7).
// Note that LLVM additionally denormalizes resource consumption to include
// usage of super resources by subresources. So in practice if there exists a
// P016 (ProcResIdx=10), then the cycles consumed by P0 are also consumed by
// P06 (ProcResIdx = 7) and P016 (ProcResIdx = 10), and the resources consumed
// by P06 are also consumed by P016. In the figure below, parenthesized cycles
// denote implied usage of superresources by subresources:
< ASCII >
//            P0      P06    P016
//     uOp1    1      (1)     (1)
//     uOp2            1      (1)
//     uOp3            1      (1)
//     =============================
//             1       3       3
< ASCII >
// Eventually we end up with three entries for the WriteProcRes of the
// instruction:
//    {ProcResIdx=1,  Cycles=1}  // P0
//    {ProcResIdx=7,  Cycles=3}  // P06
//    {ProcResIdx=10, Cycles=3}  // P016
//
// Note that in this case, P016 does not contribute any cycles, so it would
// be removed by this function.
// FIXME: Merge this with the equivalent in llvm-mca.
```
## Visual type:
- #table


== ./llvm/llvm_124.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/tools/llvm-profgen/PerfReader.h#L707-L725

```c
/*
   Format of unsymbolized profile:

< ASCII >
    [frame1 @ frame2 @ ...]  # If it's a CS profile
      number of entries in RangeCounter
      from_1-to_1:count_1
      from_2-to_2:count_2
      ......
      from_n-to_n:count_n
      number of entries in BranchCounter
      src_1->dst_1:count_1
      src_2->dst_2:count_2
      ......
      src_n->dst_n:count_n
    [frame1 @ frame2 @ ...]  # Next context
      ......
< ASCII >

Note that non-CS profile doesn't have the empty `[]` context.
*/
```
## Visual type:
- #custom


== ./llvm/llvm_125.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/tools/llvm-xray/xray-stacks.cpp#L139-L253

```c
/// The stack command will take a set of XRay traces as arguments, and collects
/// information about the stacks of instrumented functions that appear in the
/// traces. We track the following pieces of information:
///
///   - Total time: amount of time/cycles accounted for in the traces.
///   - Stack count: number of times a specific stack appears in the
///     traces. Only instrumented functions show up in stacks.
///   - Cumulative stack time: amount of time spent in a stack accumulated
///     across the invocations in the traces.
///   - Cumulative local time: amount of time spent in each instrumented
///     function showing up in a specific stack, accumulated across the traces.
///
/// Example output for the kind of data we'd like to provide looks like the
/// following:
///
< ASCII >
///   Total time: 3.33234 s
///   Stack ID: ...
///   Stack Count: 2093
///   #     Function                  Local Time     (%)      Stack Time     (%)
///   0     main                         2.34 ms   0.07%      3.33234  s    100%
///   1     foo()                     3.30000  s  99.02%         3.33  s  99.92%
///   2     bar()                          30 ms   0.90%           30 ms   0.90%
< ASCII >
///
/// We can also show distributions of the function call durations with
/// statistics at each level of the stack. This works by doing the following
/// algorithm:
///
< ASCII >
///   1. When unwinding, record the duration of each unwound function associated
///   with the path up to which the unwinding stops. For example:
///
///        Step                         Duration (? means has start time)
///
///        push a <start time>           a = ?
///        push b <start time>           a = ?, a->b = ?
///        push c <start time>           a = ?, a->b = ?, a->b->c = ?
///        pop  c <end time>             a = ?, a->b = ?, emit duration(a->b->c)
///        pop  b <end time>             a = ?, emit duration(a->b)
///        push c <start time>           a = ?, a->c = ?
///        pop  c <end time>             a = ?, emit duration(a->c)
///        pop  a <end time>             emit duration(a)
///
///   2. We then account for the various stacks we've collected, and for each of
///      them will have measurements that look like the following (continuing
///      with the above simple example):
///
///        c : [<id("a->b->c"), [durations]>, <id("a->c"), [durations]>]
///        b : [<id("a->b"), [durations]>]
///        a : [<id("a"), [durations]>]
< ASCII >
///
///      This allows us to compute, for each stack id, and each function that
///      shows up in the stack,  some important statistics like:
///
///        - median
///        - 99th percentile
///        - mean + stddev
///        - count
///
///   3. For cases where we don't have durations for some of the higher levels
///   of the stack (perhaps instrumentation wasn't activated when the stack was
///   entered), we can mark them appropriately.
///
///  Computing this data also allows us to implement lookup by call stack nodes,
///  so that we can find functions that show up in multiple stack traces and
///  show the statistical properties of that function in various contexts. We
///  can compute information similar to the following:
///
< ASCII >
///    Function: 'c'
///    Stacks: 2 / 2
///    Stack ID: ...
///    Stack Count: ...
///    #     Function  ...
///    0     a         ...
///    1     b         ...
///    2     c         ...
///
///    Stack ID: ...
///    Stack Count: ...
///    #     Function  ...
///    0     a         ...
///    1     c         ...
///    ----------------...
///
///    Function: 'b'
///    Stacks:  1 / 2
///    Stack ID: ...
///    Stack Count: ...
///    #     Function  ...
///    0     a         ...
///    1     b         ...
///    2     c         ...
< ASCII >
///
///
/// To do this we require a Trie data structure that will allow us to represent
/// all the call stacks of instrumented functions in an easily traversible
/// manner when we do the aggregations and lookups. For instrumented call
/// sequences like the following:
///
///   a()
///    b()
///     c()
///     d()
///    c()
///
/// We will have a representation like so:
///
< ASCII >
///   a -> b -> c
///   |    |
///   |    +--> d
///   |
///   +--> c
< ASCII >
///
/// We maintain a sequence of durations on the leaves and in the internal nodes
/// as we go through and process every record from the XRay trace. We also
/// maintain an index of unique functions, and provide a means of iterating
/// through all the instrumented call stacks which we know about.
```
## Visual type:
- #table


== ./llvm/llvm_126.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/tools/obj2yaml/macho2yaml.cpp#L475-L534

```c
/*!
 * /brief processes a node from the export trie, and its children.
 *
 * To my knowledge there is no documentation of the encoded format of this data
 * other than in the heads of the Apple linker engineers. To that end hopefully
 * this comment and the implementation below can serve to light the way for
 * anyone crazy enough to come down this path in the future.
 *
 * This function reads and preserves the trie structure of the export trie. To
 * my knowledge there is no code anywhere else that reads the data and preserves
 * the Trie. LD64 (sources available at opensource.apple.com) has a similar
 * implementation that parses the export trie into a vector. That code as well
 * as LLVM's libObject MachO implementation were the basis for this.
 *
 * The export trie is an encoded trie. The node serialization is a bit awkward.
 * The below pseudo-code is the best description I've come up with for it.
 *
 * struct SerializedNode {
 *   ULEB128 TerminalSize;
 *   struct TerminalData { <-- This is only present if TerminalSize > 0
 *     ULEB128 Flags;
 *     ULEB128 Address; <-- Present if (! Flags & REEXPORT )
 *     ULEB128 Other; <-- Present if ( Flags & REEXPORT ||
 *                                     Flags & STUB_AND_RESOLVER )
 *     char[] ImportName; <-- Present if ( Flags & REEXPORT )
 *   }
 *   uint8_t ChildrenCount;
 *   Pair<char[], ULEB128> ChildNameOffsetPair[ChildrenCount];
 *   SerializedNode Children[ChildrenCount]
 * }
 *
 * Terminal nodes are nodes that represent actual exports. They can appear
 * anywhere in the tree other than at the root; they do not need to be leaf
 * nodes. When reading the data out of the trie this routine reads it in-order,
 * but it puts the child names and offsets directly into the child nodes. This
 * results in looping over the children twice during serialization and
 * de-serialization, but it makes the YAML representation more human readable.
 *
 * Below is an example of the graph from a "Hello World" executable:
 *
< ASCII >
 * -------
 * | ''  |
 * -------
 *    |
 * -------
 * | '_' |
 * -------
 *    |
 *    |----------------------------------------|
 *    |                                        |
 *  ------------------------      ---------------------
 *  | '_mh_execute_header' |      | 'main'            |
 *  | Flags: 0x00000000    |      | Flags: 0x00000000 |
 *  | Addr:  0x00000000    |      | Addr:  0x00001160 |
 *  ------------------------      ---------------------
< ASCII >
 *
 * This graph represents the trie for the exports "__mh_execute_header" and
 * "_main". In the graph only the "_main" and "__mh_execute_header" nodes are
 * terminal.
*/
```
## Visual type:
- #program-graph


== ./llvm/llvm_127.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/unittests/Analysis/LazyCallGraphTest.cpp#L42-L58

```c
/*
   IR forming a call graph with a diamond of triangle-shaped SCCs:

< ASCII >
           d1
          /  \
         d3--d2
        /     \
       b1     c1
     /  \    /  \
    b3--b2  c3--c2
         \  /
          a1
         /  \
        a3--a2
< ASCII >

   All call edges go up between SCCs, and clockwise around the SCC.
 */
```
## Visual type:
- #call-graph


== ./llvm/llvm_128.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/unittests/Analysis/LazyCallGraphTest.cpp#L125-L141

```c
/*
   IR forming a reference graph with a diamond of triangle-shaped RefSCCs

< ASCII >
           d1
          /  \
         d3--d2
        /     \
       b1     c1
     /  \    /  \
    b3--b2  c3--c2
         \  /
          a1
         /  \
        a3--a2
< ASCII >

   All call edges go up between RefSCCs, and clockwise around the RefSCC.
 */
```
## Visual type:
- #call-graph


== ./llvm/llvm_129.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/unittests/Analysis/MemorySSATest.cpp#L689-L704

```c
// Bug: During phi optimization, the walker wouldn't cache to the proper result
// in the farthest-walked BB.
//
// Specifically, it would assume that whatever we walked to was a clobber.
// "Whatever we walked to" isn't a clobber if we hit a cache entry.
//
< ASCII >
// ...So, we need a test case that looks like:
//    A
//   / \
//  B   |
//   \ /
//    C
< ASCII >
//
// Where, when we try to optimize a thing in 'C', a blocker is found in 'B'.
// The walk must determine that the blocker exists by using cache entries *while
// walking* 'B'.
```
## Visual type:
- #call-graph


== ./llvm/llvm_13.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libcxxabi/src/aix_state_tab_eh.inc#L21-L65

```c
/*
  The legacy IBM xlC and xlclang++ compilers use the state table for EH
  instead of the range table. Destructors, or addresses of the possible catch
  sites or cleanup code are specified in the state table which is a finite
  state machine (FSM). Each function that has a state table also has an
  autolocal state variable. The state variable represents the current state
  of the function for EH and is found through the traceback table of the
  function during unwinding, which is located at the end of each function.
  The FSM is an array of state entries. Each state entry has the following
  fields:

  * offset/address/pointer - the offset used to locate the object, or the
    address of a global object, or the address of the next state if it is an
    old conditional state change entry;
  * dtor/landing pad - address of the destructor function to invoke,
    or address of the catch block or cleanup code in the user code to branch to;
  * element count/action flag - the number of elements or the flag for actions;
  * element size - if the object is an array this is the size of one element
    of the array;
  * flags - flags used to control how fields in the entry are interpreted;
  * next state - the state to execute next after the action for this state is
    performed. The value of zero indicates the end of the state for this
    function.

  The following is the description of 'element count/action flag' field.
< ASCII >
+-----------------------------------------------------------------------------+
| value |      description       |                  action                    |
+-------+------------------------+--------------------------------------------+
| > 1   |   object is an array   | calls __cxa_vec_cleanup to run dtor for    |
|       |                        | each member of the array                   |
+-------+------------------------+--------------------------------------------+
| 1, 0  |   object is a scalar   | calls dtor for the object                  |
+-------+------------------------+--------------------------------------------+
|  -1   |      begin catch       | branches to the handler which performes    |
|       |                        | catch-match. If there is no catch that     |
|       |                        | matches the exception it will be rethrown  |
+-------+------------------------+--------------------------------------------+
|  -2   |       end catch        | ends current catch block and continues     |
|       |                        | attempting to catch the exception          |
+-------+------------------------+--------------------------------------------+
|  -3   |   delete the object    | calls the delete function of the object    |
+-------+------------------------+--------------------------------------------+
|  -4   |      cleanup label     | branches to the user code for cleaning up  |
+-------+------------------------+--------------------------------------------+
< ASCII >
*/
```
## Visual type:
- #table


== ./llvm/llvm_130.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/unittests/Analysis/MemorySSATest.cpp#L1309-L1323

```c
< ASCII >
//   entry
//     |
//   header
//    / \
// body  |
//    \ /
//    exit
< ASCII >
// header:
//  ; 1 = MemoryDef(liveOnEntry)
// body:
//  ; 2 = MemoryDef(1)
// exit:
//  ; 3 = MemoryPhi({body, 2}, {header, 1})
//  ; 4 = MemoryDef(3); optimized to 3, cannot optimize thorugh phi.
//  Insert edge: entry -> exit, check mssa Update is correct.
```
## Visual type:
- #call-graph


== ./llvm/llvm_131.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/unittests/Analysis/MemorySSATest.cpp#L1428-L1451

```c
< ASCII >
//   entry
//    /  |
//   a   |
//  / \  |
//  b c  f
//  \ /  |
//   d   |
//    \ /
//     e
< ASCII >
// f:
//  ; 1 = MemoryDef(liveOnEntry)
// e:
//  ; 2 = MemoryPhi({d, liveOnEntry}, {f, 1})
//
// Insert edge: f -> c, check update is correct.
// After update:
// f:
//  ; 1 = MemoryDef(liveOnEntry)
// c:
//  ; 3 = MemoryPhi({a, liveOnEntry}, {f, 1})
// d:
//  ; 4 = MemoryPhi({b, liveOnEntry}, {c, 3})
// e:
//  ; 2 = MemoryPhi({d, 4}, {f, 1})
```
## Visual type:
- #call-graph


== ./llvm/llvm_132.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/unittests/IR/DominatorTreeTest.cpp#L443-L484

```c
// Verify that the PDT is correctly updated in case an edge removal results
// in an infinite loop. Also make sure that the updated PDT is the
// same as a freshly recalculated one.
//
< ASCII >
// Test case:
//
//          CFG                   PDT
//
//           A                    Exit
//           |                     |
//          _B                     D
//         / | \                   |
//        ^  v  \                  B
//        \ /    D                / \
//         C      \              C   A
//        / \      v
//       ^  v      Exit
//        \_/
//
// After deleting the edge C->B, C is part of an infinite reverse-unreachable
// loop:
//
//          CFG'                  PDT'
//
//           A                    Exit
//           |                   / | \
//           B                  C  B  D
//           | \                   |
//           v  \                  A
//          /    D
//         C      \
//        / \      v
//       ^  v      Exit
//        \_/
< ASCII >
//
// As C now becomes reverse-unreachable, it forms a new non-trivial root and
// gets connected to the virtual exit.
// D does not postdominate B anymore, because there are two forward paths from
// B to the virtual exit:
//  - B -> C -> VirtualExit
//  - B -> D -> VirtualExit.
//
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_133.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/unittests/IR/DominatorTreeTest.cpp#L535-L571

```c
// Verify that the PDT is correctly updated in case an edge removal results
// in an infinite loop.
//
< ASCII >
// Test case:
//
//          CFG                   PDT
//
//           A                    Exit
//           |                   / | \
//           B--               C2  B  D
//           |  \              /   |
//           v   \            C    A
//          /     D
//         C--C2   \
//        / \  \    v
//       ^  v  --Exit
//        \_/
//
// After deleting the edge C->E, C is part of an infinite reverse-unreachable
// loop:
//
//          CFG'                  PDT'
//
//           A                    Exit
//           |                   / | \
//           B                  C  B  D
//           | \                   |
//           v  \                  A
//          /    D
//         C      \
//        / \      v
//       ^  v      Exit
//        \_/
< ASCII >
//
// In PDT, D does not post-dominate B. After the edge C -> C2 is removed,
// C becomes a new nontrivial PDT root.
//
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_134.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/unittests/IR/DominatorTreeTest.cpp#L630-L647

```c
// Verify that the IDF returns blocks in a deterministic way.
//
< ASCII >
// Test case:
//
//          CFG
//
//          (A)
//          / \
//         /   \
//       (B)   (C)
//        |\   /|
//        |  X  |
//        |/   \|
//       (D)   (E)
< ASCII >
//
// IDF for block B is {D, E}, and the order of blocks in this list is defined by
// their 1) level in dom-tree and 2) DFSIn number if the level is the same.
//
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_135.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/examples/toy/Ch6/mlir/LowerToLLVM.cpp#L1-L23

```c
//====- LowerToLLVM.cpp - Lowering from Toy+Affine+Std to LLVM ------------===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// This file implements full lowering of Toy operations to LLVM MLIR dialect.
// 'toy.print' is lowered to a loop nest that calls `printf` on each element of
// the input array. The file also sets up the ToyToLLVMLoweringPass. This pass
// lowers the combination of Arithmetic + Affine + SCF + Func dialects to the
// LLVM one:
//
< ASCII >
//                         Affine --
//                                  |
//                                  v
//                       Arithmetic + Func --> LLVM (Dialect)
//                                  ^
//                                  |
//     'toy.print' --> Loop (SCF) --
< ASCII >
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #flowchart


== ./llvm/llvm_136.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/examples/toy/Ch7/mlir/LowerToLLVM.cpp#L1-L23

```c
//====- LowerToLLVM.cpp - Lowering from Toy+Affine+Std to LLVM ------------===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// This file implements full lowering of Toy operations to LLVM MLIR dialect.
// 'toy.print' is lowered to a loop nest that calls `printf` on each element of
// the input array. The file also sets up the ToyToLLVMLoweringPass. This pass
// lowers the combination of Arithmetic + Affine + SCF + Func dialects to the
// LLVM one:
//
< ASCII >
//                         Affine --
//                                  |
//                                  v
//                       Arithmetic + Func --> LLVM (Dialect)
//                                  ^
//                                  |
//     'toy.print' --> Loop (SCF) --
< ASCII >
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #flowchart


== ./llvm/llvm_137.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/include/mlir/Analysis/SliceAnalysis.h#L30-L70

```c
/// Fills `forwardSlice` with the computed forward slice (i.e. all
/// the transitive uses of op), **without** including that operation.
///
/// This additionally takes a TransitiveFilter which acts as a frontier:
/// when looking at uses transitively, an operation that does not pass the
/// filter is never propagated through. This allows in particular to carve out
/// the scope within a ForOp or the scope within an IfOp.
///
/// The implementation traverses the use chains in postorder traversal for
/// efficiency reasons: if an operation is already in `forwardSlice`, no
/// need to traverse its uses again. Since use-def chains form a DAG, this
/// terminates.
///
/// Upon return to the root call, `forwardSlice` is filled with a
/// postorder list of uses (i.e. a reverse topological order). To get a proper
/// topological order, we just just reverse the order in `forwardSlice` before
/// returning.
///
/// Example starting from node 0
/// ============================
///
< ASCII >
///               0
///    ___________|___________
///    1       2      3      4
///    |_______|      |______|
///    |   |             |
///    |   5             6
///    |___|_____________|
///      |               |
///      7               8
///      |_______________|
///              |
///              9
< ASCII >
///
/// Assuming all local orders match the numbering order:
/// 1. after getting back to the root getForwardSlice, `forwardSlice` may
///    contain:
///      {9, 7, 8, 5, 1, 2, 6, 3, 4}
/// 2. reversing the result of 1. gives:
///      {4, 3, 6, 2, 1, 5, 8, 7, 9}
///
```
## Visual type:
- #custom


== ./llvm/llvm_138.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/include/mlir/Analysis/SliceAnalysis.h#L79-L112

```c
/// Fills `backwardSlice` with the computed backward slice (i.e.
/// all the transitive defs of op), **without** including that operation.
///
/// This additionally takes a TransitiveFilter which acts as a frontier:
/// when looking at defs transitively, an operation that does not pass the
/// filter is never propagated through. This allows in particular to carve out
/// the scope within a ForOp or the scope within an IfOp.
///
/// The implementation traverses the def chains in postorder traversal for
/// efficiency reasons: if an operation is already in `backwardSlice`, no
/// need to traverse its definitions again. Since useuse-def chains form a DAG,
/// this terminates.
///
/// Upon return to the root call, `backwardSlice` is filled with a
/// postorder list of defs. This happens to be a topological order, from the
/// point of view of the use-def chains.
///
/// Example starting from node 8
/// ============================
///
< ASCII >
///    1       2      3      4
///    |_______|      |______|
///    |   |             |
///    |   5             6
///    |___|_____________|
///      |               |
///      7               8
///      |_______________|
///              |
///              9
< ASCII >
///
/// Assuming all local orders match the numbering order:
///    {1, 2, 5, 3, 4, 6}
///
```
## Visual type:
- #custom


== ./llvm/llvm_139.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/include/mlir/Analysis/SliceAnalysis.h#L121-L197

```c
/// Iteratively computes backward slices and forward slices until
/// a fixed point is reached. Returns an `SetVector<Operation *>` which
/// **includes** the original operation.
///
/// This allows building a slice (i.e. multi-root DAG where everything
/// that is reachable from an Value in forward and backward direction is
/// contained in the slice).
/// This is the abstraction we need to materialize all the operations for
/// supervectorization without worrying about orderings and Value
/// replacements.
///
/// Example starting from any node
/// ==============================
///
< ASCII >
///    1       2      3      4
///    |_______|      |______|
///    |   |             |   |
///    |   5             6___|
///    |___|_____________|   |
///      |               |   |
///      7               8   |
///      |_______________|   |
///              |           |
///              9          10
< ASCII >
///
/// Return the whole DAG in some topological order.
///
/// The implementation works by just filling up a worklist with iterative
/// alternate calls to `getBackwardSlice` and `getForwardSlice`.
///
/// The following section describes some additional implementation
/// considerations for a potentially more efficient implementation but they are
/// just an intuition without proof, we still use a worklist for now.
///
/// Additional implementation considerations
/// ========================================
< ASCII >
/// Consider the defs-op-uses hourglass.
///    ____
///    \  /  defs (in some topological order)
///     \/
///     op
///     /\
///    /  \  uses (in some topological order)
///   /____\
< ASCII >
///
/// We want to iteratively apply `getSlice` to construct the whole
/// list of Operation that are reachable by (use|def)+ from op.
/// We want the resulting slice in topological order.
/// Ideally we would like the ordering to be maintained in-place to avoid
/// copying Operation at each step. Keeping this ordering by construction
/// seems very unclear, so we list invariants in the hope of seeing whether
/// useful properties pop up.
///
/// In the following:
///   we use |= for set inclusion;
///   we use << for set topological ordering (i.e. each pair is ordered).
///
/// Assumption:
/// ===========
/// We wish to maintain the following property by a recursive argument:
///   """
///      defs << {op} <<uses are in topological order.
///   """
/// The property clearly holds for 0 and 1-sized uses and defs;
///
/// Invariants:
///   2. defs and uses are in topological order internally, by construction;
///   3. for any {x} |= defs, defs(x) |= defs;    because all go through op
///   4. for any {x} |= uses,    defs |= defs(x); because all go through op
///   5. for any {x} |= defs,    uses |= uses(x); because all go through op
///   6. for any {x} |= uses, uses(x) |= uses;    because all go through op
///
/// Intuitively, we should be able to recurse like:
///   preorder(defs) - op - postorder(uses)
/// and keep things ordered but this is still hand-wavy and not worth the
/// trouble for now: punt to a simple worklist-based solution.
///
```
## Visual type:
- #custom


== ./llvm/llvm_14.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libcxxabi/src/cxa_guard_impl.h#L12-L45

```c
/* cxa_guard_impl.h - Implements the C++ runtime support for function local
 * static guards.
 * The layout of the guard object is the same across ARM and Itanium.
 *
 * The first "guard byte" (which is checked by the compiler) is set only upon
 * the completion of cxa release.
 *
 * The second "init byte" does the rest of the bookkeeping. It tracks if
 * initialization is complete or pending, and if there are waiting threads.
 *
 * If the guard variable is 64-bits and the platforms supplies a 32-bit thread
 * identifier, it is used to detect recursive initialization. The thread ID of
 * the thread currently performing initialization is stored in the second word.
 *
< ASCII >
 *  Guard Object Layout:
 * ---------------------------------------------------------------------------
 * | a+0: guard byte | a+1: init byte | a+2: unused ... | a+4: thread-id ... |
 * ---------------------------------------------------------------------------
< ASCII >
 *
 * Note that we don't do what the ABI docs suggest (put a mutex in the guard
 * object which we acquire in cxa_guard_acquire and release in
 * cxa_guard_release). Instead we use the init byte to imitate that behaviour,
 * but without actually holding anything mutex related between aquire and
 * release/abort.
 *
 *  Access Protocol:
 *    For each implementation the guard byte is checked and set before accessing
 *    the init byte.
 *
 *  Overall Design:
 *    The implementation was designed to allow each implementation to be tested
 *    independent of the C++ runtime or platform support.
 *
 */
```
## Visual type:
- #memory-layout


== ./llvm/llvm_140.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/include/mlir/Dialect/Vector/Transforms/VectorRewritePatterns.h#L281-L310

```c
/// Collect a set of pattern to unroll vector operations to a smaller shapes.
/// `options` structure controls which operations are unrolled and the target
/// shape.
/// `op` is unrolled to the `targetShape` as follows, for each of its operands:
///   1. the unrolled type `unrolledVectorType` and number of unrolled instances
///   `numUnrolledInstances` are computed from the `targetShape`. For now it is
///   assumed the unrolling factors divide the vector sizes.
///   2. ExtractStridedSlice are created to break-up the vector operands.
///   3. the original op is cloned `numUnrolledInstances` times, once for each
///   result.
///   4. InsertStridedSlice are inserted to re-assemble the slices into the
///   original vectore shape.
///
/// Example:
///
< ASCII >
///    opA(operand0, operand1)  // numUnrolledInstances = 3
///
///            operand0                   operand1
///               |                          |
///             fork                       fork
///        <----------gather all fork ops --------->
///              /|\                        /|\
///          f00 f01 f02                f10 f11 f12
///        <---------- clone op 3 times --------->
///          opA0(f00, f10), opA1(f01, f11), opA2(f02, f12)
///                 \            |            /
///      <-------------------- join ------------------------->
< ASCII >
///
/// Other local patterns then kick in iteratively (including DCE) and compose
/// to combine the ExtractStridedSlice/InsertStridedSlice.
```
## Visual type:
- #program-graph


== ./llvm/llvm_141.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Analysis/Presburger/PresburgerRelation.cpp#L680-L695

```c
/// Given two polyhedra `a` and `b` at positions `i` and `j` in
/// `disjuncts` and `redundantIneqsA` being the inequalities of `a` that
/// are redundant for `b` (similarly for `cuttingIneqsA`, `redundantIneqsB`,
/// and `cuttingIneqsB`), Checks whether the facets of all cutting
/// inequalites of `a` are contained in `b`. If so, a new polyhedron
/// consisting of all redundant inequalites of `a` and `b` and all
/// equalities of both is created.
///
< ASCII >
/// An example of this case:
///    ___________        ___________
///   /   /  |   /       /          /
///   \   \  |  /   ==>  \         /
///    \   \ | /          \       /
///     \___\|/            \_____/
< ASCII >
///
///
```
## Visual type:
- #custom


== ./llvm/llvm_142.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Conversion/MemRefToLLVM/MemRefToLLVM.cpp#L498-L524

```c
/// Wrap a llvm.cmpxchg operation in a while loop so that the operation can be
/// retried until it succeeds in atomically storing a new value into memory.
///
< ASCII >
///      +---------------------------------+
///      |   <code before the AtomicRMWOp> |
///      |   <compute initial %loaded>     |
///      |   cf.br loop(%loaded)              |
///      +---------------------------------+
///             |
///  -------|   |
///  |      v   v
///  |   +--------------------------------+
///  |   | loop(%loaded):                 |
///  |   |   <body contents>              |
///  |   |   %pair = cmpxchg              |
///  |   |   %ok = %pair[0]               |
///  |   |   %new = %pair[1]              |
///  |   |   cf.cond_br %ok, end, loop(%new) |
///  |   +--------------------------------+
///  |          |        |
///  |-----------        |
///                      v
///      +--------------------------------+
///      | end:                           |
///      |   <code after the AtomicRMWOp> |
///      +--------------------------------+
< ASCII >
///
```
## Visual type:
- #program-graph


== ./llvm/llvm_143.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Conversion/PDLToPDLInterp/RootOrdering.h#L31-L58

```c
/// The information associated with an edge in the cost graph. Each node in
/// the cost graph corresponds to a candidate root detected in the pdl.pattern,
/// and each edge in the cost graph corresponds to connecting the two candidate
/// roots via a chain of operations. The cost of an edge is the smallest number
/// of upward traversals required to go from the source to the target root, and
/// the connector is a `Value` in the intersection of the two subtrees rooted at
/// the source and target root that results in that smallest number of upward
/// traversals. Consider the following pattern with 3 roots op3, op4, and op5:
///
< ASCII >
///                 argA ---> op1 ---> op2 ---> op3 ---> res3
///                            ^        ^
///                            |        |
///                           argB     argC
///                            |        |
///                            v        v
///                 res4 <--- op4      op5 ---> res5
///                            ^        ^
///                            |        |
///                           op6      op7
< ASCII >
///
/// The cost of the edge op3 -> op4 is 1 (the upward traversal argB -> op4),
/// with argB being the connector `Value` and similarly for op3 -> op5 (cost 1,
/// connector argC). The cost of the edge op4 -> op3 is 3 (upward traversals
/// argB -> op1 -> op2 -> op3, connector argB), while the cost of edge op5 ->
/// op3 is 2 (uwpard traversals argC -> op2 -> op3). There are no edges between
/// op4 and op5 in the cost graph, because the subtrees rooted at these two
/// roots do not intersect. It is easy to see that the optimal root for this
/// pattern is op3, resulting in the spanning arborescence op3 -> {op4, op5}.
```
## Visual type:
- #program-graph


== ./llvm/llvm_144.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Conversion/SCFToControlFlow/SCFToControlFlow.cpp#L42-L102

```c
// Create a CFG subgraph for the loop around its body blocks (if the body
// contained other loops, they have been already lowered to a flow of blocks).
// Maintain the invariants that a CFG subgraph created for any loop has a single
// entry and a single exit, and that the entry/exit blocks are respectively
// first/last blocks in the parent region.  The original loop operation is
// replaced by the initialization operations that set up the initial value of
// the loop induction variable (%iv) and computes the loop bounds that are loop-
// invariant for affine loops.  The operations following the original scf.for
// are split out into a separate continuation (exit) block. A condition block is
// created before the continuation block. It checks the exit condition of the
// loop and branches either to the continuation block, or to the first block of
// the body. The condition block takes as arguments the values of the induction
// variable followed by loop-carried values. Since it dominates both the body
// blocks and the continuation block, loop-carried values are visible in all of
// those blocks. Induction variable modification is appended to the last block
// of the body (which is the exit block from the body subgraph thanks to the
// invariant we maintain) along with a branch that loops back to the condition
// block. Loop-carried values are the loop terminator operands, which are
// forwarded to the branch.
//
< ASCII >
//      +---------------------------------+
//      |   <code before the ForOp>       |
//      |   <definitions of %init...>     |
//      |   <compute initial %iv value>   |
//      |   cf.br cond(%iv, %init...)        |
//      +---------------------------------+
//             |
//  -------|   |
//  |      v   v
//  |   +--------------------------------+
//  |   | cond(%iv, %init...):           |
//  |   |   <compare %iv to upper bound> |
//  |   |   cf.cond_br %r, body, end        |
//  |   +--------------------------------+
//  |          |               |
//  |          |               -------------|
//  |          v                            |
//  |   +--------------------------------+  |
//  |   | body-first:                    |  |
//  |   |   <%init visible by dominance> |  |
//  |   |   <body contents>              |  |
//  |   +--------------------------------+  |
//  |                   |                   |
//  |                  ...                  |
//  |                   |                   |
//  |   +--------------------------------+  |
//  |   | body-last:                     |  |
//  |   |   <body contents>              |  |
//  |   |   <operands of yield = %yields>|  |
//  |   |   %new_iv =<add step to %iv>   |  |
//  |   |   cf.br cond(%new_iv, %yields)    |  |
//  |   +--------------------------------+  |
//  |          |                            |
//  |-----------        |--------------------
//                      v
//      +--------------------------------+
//      | end:                           |
//      |   <code after the ForOp>       |
//      |   <%init visible by dominance> |
//      +--------------------------------+
< ASCII >
//
```
## Visual type:
- #program-graph


== ./llvm/llvm_145.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Conversion/SCFToControlFlow/SCFToControlFlow.cpp#L110-L195

```c
// Create a CFG subgraph for the scf.if operation (including its "then" and
// optional "else" operation blocks).  We maintain the invariants that the
// subgraph has a single entry and a single exit point, and that the entry/exit
// blocks are respectively the first/last block of the enclosing region. The
// operations following the scf.if are split into a continuation (subgraph
// exit) block. The condition is lowered to a chain of blocks that implement the
// short-circuit scheme. The "scf.if" operation is replaced with a conditional
// branch to either the first block of the "then" region, or to the first block
// of the "else" region. In these blocks, "scf.yield" is unconditional branches
// to the post-dominating block. When the "scf.if" does not return values, the
// post-dominating block is the same as the continuation block. When it returns
// values, the post-dominating block is a new block with arguments that
// correspond to the values returned by the "scf.if" that unconditionally
// branches to the continuation block. This allows block arguments to dominate
// any uses of the hitherto "scf.if" results that they replaced. (Inserting a
// new block allows us to avoid modifying the argument list of an existing
// block, which is illegal in a conversion pattern). When the "else" region is
// empty, which is only allowed for "scf.if"s that don't return values, the
// condition branches directly to the continuation block.
//
< ASCII >
// CFG for a scf.if with else and without results.
//
//      +--------------------------------+
//      | <code before the IfOp>         |
//      | cf.cond_br %cond, %then, %else    |
//      +--------------------------------+
//             |              |
//             |              --------------|
//             v                            |
//      +--------------------------------+  |
//      | then:                          |  |
//      |   <then contents>              |  |
//      |   cf.br continue                  |  |
//      +--------------------------------+  |
//             |                            |
//   |----------               |-------------
//   |                         V
//   |  +--------------------------------+
//   |  | else:                          |
//   |  |   <else contents>              |
//   |  |   cf.br continue                  |
//   |  +--------------------------------+
//   |         |
//   ------|   |
//         v   v
//      +--------------------------------+
//      | continue:                      |
//      |   <code after the IfOp>        |
//      +--------------------------------+
//
// CFG for a scf.if with results.
//
//      +--------------------------------+
//      | <code before the IfOp>         |
//      | cf.cond_br %cond, %then, %else    |
//      +--------------------------------+
//             |              |
//             |              --------------|
//             v                            |
//      +--------------------------------+  |
//      | then:                          |  |
//      |   <then contents>              |  |
//      |   cf.br dom(%args...)             |  |
//      +--------------------------------+  |
//             |                            |
//   |----------               |-------------
//   |                         V
//   |  +--------------------------------+
//   |  | else:                          |
//   |  |   <else contents>              |
//   |  |   cf.br dom(%args...)             |
//   |  +--------------------------------+
//   |         |
//   ------|   |
//         v   v
//      +--------------------------------+
//      | dom(%args...):                 |
//      |   cf.br continue                  |
//      +--------------------------------+
//             |
//             v
//      +--------------------------------+
//      | continue:                      |
//      | <code after the IfOp>          |
//      +--------------------------------+
< ASCII >
//
```
## Visual type:
- #program-graph


== ./llvm/llvm_146.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Conversion/SCFToControlFlow/SCFToControlFlow.cpp#L217-L275

```c
/// Create a CFG subgraph for this loop construct. The regions of the loop need
/// not be a single block anymore (for example, if other SCF constructs that
/// they contain have been already converted to CFG), but need to be single-exit
/// from the last block of each region. The operations following the original
/// WhileOp are split into a new continuation block. Both regions of the WhileOp
/// are inlined, and their terminators are rewritten to organize the control
/// flow implementing the loop as follows.
///
< ASCII >
///      +---------------------------------+
///      |   <code before the WhileOp>     |
///      |   cf.br ^before(%operands...)      |
///      +---------------------------------+
///             |
///  -------|   |
///  |      v   v
///  |   +--------------------------------+
///  |   | ^before(%bargs...):            |
///  |   |   %vals... = <some payload>    |
///  |   +--------------------------------+
///  |                   |
///  |                  ...
///  |                   |
///  |   +--------------------------------+
///  |   | ^before-last:
///  |   |   %cond = <compute condition>  |
///  |   |   cf.cond_br %cond,               |
///  |   |        ^after(%vals...), ^cont |
///  |   +--------------------------------+
///  |          |               |
///  |          |               -------------|
///  |          v                            |
///  |   +--------------------------------+  |
///  |   | ^after(%aargs...):             |  |
///  |   |   <body contents>              |  |
///  |   +--------------------------------+  |
///  |                   |                   |
///  |                  ...                  |
///  |                   |                   |
///  |   +--------------------------------+  |
///  |   | ^after-last:                   |  |
///  |   |   %yields... = <some payload>  |  |
///  |   |   cf.br ^before(%yields...)       |  |
///  |   +--------------------------------+  |
///  |          |                            |
///  |-----------        |--------------------
///                      v
///      +--------------------------------+
///      | ^cont:                         |
///      |   <code after the WhileOp>     |
///      |   <%vals from 'before' region  |
///      |          visible by dominance> |
///      +--------------------------------+
< ASCII >
///
/// Values are communicated between ex-regions (the groups of blocks that used
/// to form a region before inlining) through block arguments of their
/// entry blocks, which are visible in all other dominated blocks. Similarly,
/// the results of the WhileOp are defined in the 'before' region, which is
/// required to have a single existing block, and are therefore accessible in
/// the continuation block due to dominance.
```
## Visual type:
- #program-graph


== ./llvm/llvm_147.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Conversion/SPIRVToLLVM/SPIRVToLLVM.cpp#L994-L1041

```c
/// Converts `spirv.mlir.loop` to LLVM dialect. All blocks within selection
/// should be reachable for conversion to succeed. The structure of the loop in
/// LLVM dialect will be the following:
///
< ASCII >
///      +------------------------------------+
///      | <code before spirv.mlir.loop>        |
///      | llvm.br ^header                    |
///      +------------------------------------+
///                           |
///   +----------------+      |
///   |                |      |
///   |                V      V
///   |  +------------------------------------+
///   |  | ^header:                           |
///   |  |   <header code>                    |
///   |  |   llvm.cond_br %cond, ^body, ^exit |
///   |  +------------------------------------+
///   |                    |
///   |                    |----------------------+
///   |                    |                      |
///   |                    V                      |
///   |  +------------------------------------+   |
///   |  | ^body:                             |   |
///   |  |   <body code>                      |   |
///   |  |   llvm.br ^continue                |   |
///   |  +------------------------------------+   |
///   |                    |                      |
///   |                    V                      |
///   |  +------------------------------------+   |
///   |  | ^continue:                         |   |
///   |  |   <continue code>                  |   |
///   |  |   llvm.br ^header                  |   |
///   |  +------------------------------------+   |
///   |               |                           |
///   +---------------+    +----------------------+
///                        |
///                        V
///      +------------------------------------+
///      | ^exit:                             |
///      |   llvm.br ^remaining               |
///      +------------------------------------+
///                        |
///                        V
///      +------------------------------------+
///      | ^remaining:                        |
///      |   <code after spirv.mlir.loop>       |
///      +------------------------------------+
< ASCII >
///
```
## Visual type:
- #program-graph


== ./llvm/llvm_148.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Dialect/Affine/Analysis/AffineStructures.cpp#L192-L202

```c
// Construct a hyperrectangular constraint set from ValueRanges that represent
// induction variables, lower and upper bounds. `ivs`, `lbs` and `ubs` are
// expected to match one to one. The order of variables and constraints is:
//
< ASCII >
// ivs | lbs | ubs | eq/ineq
// ----+-----+-----+---------
//   1   -1     0      >= 0
// ----+-----+-----+---------
//  -1    0     1      >= 0
< ASCII >
//
// All dimensions as set as VarKind::SetDim.
```
## Visual type:
- #table


== ./llvm/llvm_149.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Dialect/Affine/Analysis/Utils.cpp#L1422-L1458

```c
// Attempt to simplify the given min/max operation by proving that its value is
// bounded by the same lower and upper bound.
//
// Bounds are computed by FlatAffineValueConstraints. Invariants required for
// finding/proving bounds should be supplied via `constraints`.
//
// 1. Add dimensions for `op` and `opBound` (lower or upper bound of `op`).
// 2. Compute an upper bound of `op` (in case of `isMin`) or a lower bound (in
//    case of `!isMin`) and bind it to `opBound`. SSA values that are used in
//    `op` but are not part of `constraints`, are added as extra symbols.
// 3. For each result of `op`: Add result as a dimension `r_i`. Prove that:
//    * If `isMin`: r_i >= opBound
//    * If `isMax`: r_i <= opBound
//    If this is the case, ub(op) == lb(op).
// 4. Replace `op` with `opBound`.
//
// In summary, the following constraints are added throughout this function.
// Note: `invar` are dimensions added by the caller to express the invariants.
// (Showing only the case where `isMin`.)
//
< ASCII >
//  invar |    op | opBound | r_i | extra syms... | const |           eq/ineq
//  ------+-------+---------+-----+---------------+-------+-------------------
//   (various eq./ineq. constraining `invar`, added by the caller)
//    ... |     0 |       0 |   0 |             0 |   ... |               ...
//  ------+-------+---------+-----+---------------+-------+-------------------
//  (various ineq. constraining `op` in terms of `op` operands (`invar` and
//    extra `op` operands "extra syms" that are not in `invar`)).
//    ... |    -1 |       0 |   0 |           ... |   ... |              >= 0
//  ------+-------+---------+-----+---------------+-------+-------------------
//   (set `opBound` to `op` upper bound in terms of `invar` and "extra syms")
//    ... |     0 |      -1 |   0 |           ... |   ... |               = 0
//  ------+-------+---------+-----+---------------+-------+-------------------
//   (for each `op` map result r_i: set r_i to corresponding map result,
//    prove that r_i >= minOpUb via contradiction)
//    ... |     0 |       0 |  -1 |           ... |   ... |               = 0
//      0 |     0 |       1 |  -1 |             0 |    -1 |              >= 0
< ASCII >
//
```
## Visual type:
- #table


== ./llvm/llvm_15.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libcxxabi/src/cxa_personality.cpp#L44-L154

```c
/*
< ASCII >
    Exception Header Layout:

+---------------------------+-----------------------------+---------------+
| __cxa_exception           | _Unwind_Exception CLNGC++\0 | thrown object |
+---------------------------+-----------------------------+---------------+
                                                          ^
                                                          |
  +-------------------------------------------------------+
  |
+---------------------------+-----------------------------+
| __cxa_dependent_exception | _Unwind_Exception CLNGC++\1 |
+---------------------------+-----------------------------+

    Exception Handling Table Layout:

+-----------------+--------+
| lpStartEncoding | (char) |
+---------+-------+--------+---------------+-----------------------+
| lpStart | (encoded with lpStartEncoding) | defaults to funcStart |
+---------+-----+--------+-----------------+---------------+-------+
| ttypeEncoding | (char) | Encoding of the type_info table |
+---------------+-+------+----+----------------------------+----------------+
| classInfoOffset | (ULEB128) | Offset to type_info table, defaults to null |
+-----------------++--------+-+----------------------------+----------------+
| callSiteEncoding | (char) | Encoding for Call Site Table |
+------------------+--+-----+-----+------------------------+--------------------------+
| callSiteTableLength | (ULEB128) | Call Site Table length, used to find Action table |
+---------------------+-----------+---------------------------------------------------+
#ifndef __USING_SJLJ_EXCEPTIONS__
+---------------------+-----------+------------------------------------------------+
| Beginning of Call Site Table            The current ip lies within the           |
| ...                                     (start, length) range of one of these    |
|                                         call sites. There may be action needed.  |
| +-------------+---------------------------------+------------------------------+ |
| | start       | (encoded with callSiteEncoding) | offset relative to funcStart | |
| | length      | (encoded with callSiteEncoding) | length of code fragment      | |
| | landingPad  | (encoded with callSiteEncoding) | offset relative to lpStart   | |
| | actionEntry | (ULEB128)                       | Action Table Index 1-based   | |
| |             |                                 | actionEntry == 0 -> cleanup  | |
| +-------------+---------------------------------+------------------------------+ |
| ...                                                                              |
+----------------------------------------------------------------------------------+
#else  // __USING_SJLJ_EXCEPTIONS__
+---------------------+-----------+------------------------------------------------+
| Beginning of Call Site Table            The current ip is a 1-based index into   |
| ...                                     this table.  Or it is -1 meaning no      |
|                                         action is needed.  Or it is 0 meaning    |
|                                         terminate.                               |
| +-------------+---------------------------------+------------------------------+ |
| | landingPad  | (ULEB128)                       | offset relative to lpStart   | |
| | actionEntry | (ULEB128)                       | Action Table Index 1-based   | |
| |             |                                 | actionEntry == 0 -> cleanup  | |
| +-------------+---------------------------------+------------------------------+ |
| ...                                                                              |
+----------------------------------------------------------------------------------+
#endif // __USING_SJLJ_EXCEPTIONS__
+---------------------------------------------------------------------+
| Beginning of Action Table       ttypeIndex == 0 : cleanup           |
| ...                             ttypeIndex  > 0 : catch             |
|                                 ttypeIndex  < 0 : exception spec    |
| +--------------+-----------+--------------------------------------+ |
| | ttypeIndex   | (SLEB128) | Index into type_info Table (1-based) | |
| | actionOffset | (SLEB128) | Offset into next Action Table entry  | |
| +--------------+-----------+--------------------------------------+ |
| ...                                                                 |
+---------------------------------------------------------------------+-----------------+
| type_info Table, but classInfoOffset does *not* point here!                           |
| +----------------+------------------------------------------------+-----------------+ |
| | Nth type_info* | Encoded with ttypeEncoding, 0 means catch(...) | ttypeIndex == N | |
| +----------------+------------------------------------------------+-----------------+ |
| ...                                                                                   |
| +----------------+------------------------------------------------+-----------------+ |
| | 1st type_info* | Encoded with ttypeEncoding, 0 means catch(...) | ttypeIndex == 1 | |
| +----------------+------------------------------------------------+-----------------+ |
| +---------------------------------------+-----------+------------------------------+  |
| | 1st ttypeIndex for 1st exception spec | (ULEB128) | classInfoOffset points here! |  |
| | ...                                   | (ULEB128) |                              |  |
| | Mth ttypeIndex for 1st exception spec | (ULEB128) |                              |  |
| | 0                                     | (ULEB128) |                              |  |
| +---------------------------------------+------------------------------------------+  |
| ...                                                                                   |
| +---------------------------------------+------------------------------------------+  |
| | 0                                     | (ULEB128) | throw()                      |  |
| +---------------------------------------+------------------------------------------+  |
| ...                                                                                   |
| +---------------------------------------+------------------------------------------+  |
| | 1st ttypeIndex for Nth exception spec | (ULEB128) |                              |  |
| | ...                                   | (ULEB128) |                              |  |
| | Mth ttypeIndex for Nth exception spec | (ULEB128) |                              |  |
| | 0                                     | (ULEB128) |                              |  |
| +---------------------------------------+------------------------------------------+  |
+---------------------------------------------------------------------------------------+
< ASCII >

Notes:

*  ttypeIndex in the Action Table, and in the exception spec table, is an index,
     not a byte count, if positive.  It is a negative index offset of
     classInfoOffset and the sizeof entry depends on ttypeEncoding.
   But if ttypeIndex is negative, it is a positive 1-based byte offset into the
     type_info Table.
   And if ttypeIndex is zero, it refers to a catch (...).

*  landingPad can be 0, this implies there is nothing to be done.

*  landingPad != 0 and actionEntry == 0 implies a cleanup needs to be done
     @landingPad.

*  A cleanup can also be found under landingPad != 0 and actionEntry != 0 in
     the Action Table with ttypeIndex == 0.
*/
```
## Visual type:
- #memory-layout


== ./llvm/llvm_150.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Dialect/SPIRV/IR/SPIRVCanonicalization.cpp#L304-L328

```c
// Blocks from the given `spirv.mlir.selection` operation must satisfy the
// following layout:
//
< ASCII >
//       +-----------------------------------------------+
//       | header block                                  |
//       | spirv.BranchConditionalOp %cond, ^case0, ^case1 |
//       +-----------------------------------------------+
//                            /   \
//                             ...
//
//
//   +------------------------+    +------------------------+
//   | case #0                |    | case #1                |
//   | spirv.Store %ptr %value0 |    | spirv.Store %ptr %value1 |
//   | spirv.Branch ^merge      |    | spirv.Branch ^merge      |
//   +------------------------+    +------------------------+
//
//
//                             ...
//                            \   /
//                              v
//                       +-------------+
//                       | merge block |
//                       +-------------+
< ASCII >
//
```
## Visual type:
- #program-graph


== ./llvm/llvm_151.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/lib/Dialect/X86Vector/Transforms/AVXTranspose.cpp#L59-L62

```c
< ASCII >
///                            a  a   b   b  a  a   b   b
/// Takes an 8 bit mask, 2 bit for each position of a[0, 3)  **and** b[0, 4):
///                                 0:127    |         128:255
///                            b01  b23  C8  D8  |  b01+4 b23+4 C8+4 D8+4
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_152.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-deallocation.mlir#L7-L16

```c
< ASCII >
// Test Case:
//    bb0
//   /   \
//  bb1  bb2 <- Initial position of AllocOp
//   \   /
//    bb3
< ASCII >
// BufferDeallocation expected behavior: bb2 contains an AllocOp which is
// passed to bb3. In the latter block, there should be an deallocation.
// Since bb1 does not contain an adequate alloc and the alloc in bb2 is not
// moved to bb0, we need to insert allocs and copies.
```
## Visual type:
- #program-graph


== ./llvm/llvm_153.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-deallocation.mlir#L46-L57

```c
< ASCII >
// Test Case:
//    bb0
//   /   \
//  bb1  bb2 <- Initial position of AllocOp
//   \   /
//    bb3
< ASCII >
// BufferDeallocation expected behavior: The existing AllocOp has a dynamic
// dependency to block argument %0 in bb2. Since the dynamic type is passed
// to bb3 via the block argument %2, it is currently required to allocate a
// temporary buffer for %2 that gets copies of %arg0 and %1 with their
// appropriate shape dimensions. The copy buffer deallocation will be applied
// to %2 in block bb3.
```
## Visual type:
- #program-graph


== ./llvm/llvm_154.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-deallocation.mlir#L130-L148

```c
< ASCII >
// Test Case:
//      bb0
//     /    \
//   bb1    bb2 <- Initial position of AllocOp
//    |     /  \
//    |   bb3  bb4
//    |     \  /
//    \     bb5
//     \    /
//       bb6
//        |
//       bb7
< ASCII >
// BufferDeallocation expected behavior: The existing AllocOp has a dynamic
// dependency to block argument %0 in bb2. Since the dynamic type is passed to
// bb5 via the block argument %2 and to bb6 via block argument %3, it is
// currently required to allocate temporary buffers for %2 and %3 that gets
// copies of %1 and %arg0 1 with their appropriate shape dimensions. The copy
// buffer deallocations will be applied to %2 in block bb5 and to %3 in block
// bb6. Furthermore, there should be no copy inserted for %4.
```
## Visual type:
- #program-graph


== ./llvm/llvm_155.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-deallocation.mlir#L216-L224

```c
< ASCII >
// Test Case:
//    bb0
//   /   \
//  |    bb1 <- Initial position of AllocOp
//   \   /
//    bb2
< ASCII >
// BufferDeallocation expected behavior: It should insert a DeallocOp at the
// exit block after CopyOp since %1 is an alias for %0 and %arg1. Furthermore,
// we have to insert a copy and an alloc in the beginning of the function.
```
## Visual type:
- #program-graph


== ./llvm/llvm_156.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-deallocation.mlir#L250-L257

```c
< ASCII >
// Test Case:
//    bb0 <- Initial position of AllocOp
//   /   \
//  |    bb1
//   \   /
//    bb2
< ASCII >
// BufferDeallocation expected behavior: It only inserts a DeallocOp at the
// exit block after CopyOp since %1 is an alias for %0 and %arg1.
```
## Visual type:
- #program-graph


== ./llvm/llvm_157.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-deallocation.mlir#L276-L285

```c
< ASCII >
// Test Case:
//    bb0 <- Initial position of the first AllocOp
//   /   \
//  bb1  bb2
//   \   /
//    bb3 <- Initial position of the second AllocOp
< ASCII >
// BufferDeallocation expected behavior: It only inserts two missing
// DeallocOps in the exit block. %5 is an alias for %0. Therefore, the
// DeallocOp for %0 should occur after the last BufferBasedOp. The Dealloc for
// %7 should happen after CopyOp.
```
## Visual type:
- #program-graph


== ./llvm/llvm_158.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-deallocation.mlir#L316-L323

```c
< ASCII >
// Test Case: No users for buffer in if-else CFG
//    bb0 <- Initial position of AllocOp
//   /   \
//  bb1  bb2
//   \   /
//    bb3
< ASCII >
// BufferDeallocation expected behavior: It only inserts a missing DeallocOp
// in the exit block since %5 or %6 are the latest aliases of %0.
```
## Visual type:
- #program-graph


== ./llvm/llvm_159.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-deallocation.mlir#L348-L358

```c
< ASCII >
// Test Case:
//      bb0 <- Initial position of the first AllocOp
//     /    \
//   bb1    bb2
//    |     /  \
//    |   bb3  bb4
//    \     \  /
//     \     /
//       bb5 <- Initial position of the second AllocOp
< ASCII >
// BufferDeallocation expected behavior: Two missing DeallocOps should be
// inserted in the exit block.
```
## Visual type:
- #program-graph


== ./llvm/llvm_16.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libcxxabi/test/dynamic_cast3.pass.cpp#L283-L289

```c
/*

< ASCII >
A1   A2
 \  /
  A3
< ASCII >

*/
```
## Visual type:
- #tree


== ./llvm/llvm_160.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-deallocation.mlir#L417-L426

```c
// Test Case:
< ASCII >
//                                     bb0
//                                    /   \
// Initial pos of the 1st AllocOp -> bb1  bb2 <- Initial pos of the 2nd AllocOp
//                                    \   /
//                                     bb3
< ASCII >
// BufferDeallocation expected behavior: We need to introduce a copy for each
// buffer since the buffers are passed to bb3. The both missing DeallocOps are
// inserted in the respective block of the allocs. The copy is freed in the exit
// block.
```
## Visual type:
- #program-graph


== ./llvm/llvm_161.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-deallocation.mlir#L467-L475

```c
// Test Case: Invalid position of the DeallocOp. There is a user after
// deallocation.
< ASCII >
//   bb0
//  /   \
// bb1  bb2 <- Initial position of AllocOp
//  \   /
//   bb3
< ASCII >
// BufferDeallocation expected behavior: The existing DeallocOp should be
// moved to exit block.
```
## Visual type:
- #program-graph


== ./llvm/llvm_162.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-hoisting.mlir#L6-L13

```c
// Test Case:
< ASCII >
//    bb0
//   /   \
//  bb1  bb2 <- Initial position of AllocOp
//   \   /
//    bb3
< ASCII >
// BufferHoisting expected behavior: It should move the existing AllocOp to
// the entry block.
```
## Visual type:
- #program-graph


== ./llvm/llvm_163.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-hoisting.mlir#L34-L42

```c
// Test Case:
< ASCII >
//    bb0
//   /   \
//  bb1  bb2 <- Initial position of AllocOp
//   \   /
//    bb3
< ASCII >
// BufferHoisting expected behavior: It should not move the existing AllocOp
// to any other block since the alloc has a dynamic dependency to block argument
// %0 in bb2.
```
## Visual type:
- #program-graph


== ./llvm/llvm_164.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-hoisting.mlir#L70-L84

```c
// Test Case:
< ASCII >
//      bb0
//     /    \
//   bb1    bb2 <- Initial position of AllocOp
//    |     /  \
//    |   bb3  bb4
//    |     \  /
//    \     bb5
//     \    /
//       bb6
//        |
//       bb7
< ASCII >
// BufferHoisting expected behavior: It should not move the existing AllocOp
// to any other block since the alloc has a dynamic dependency to block argument
// %0 in bb2.
```
## Visual type:
- #program-graph


== ./llvm/llvm_165.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-hoisting.mlir#L120-L127

```c
< ASCII >
// Test Case:
//    bb0
//   /   \
//  |    bb1 <- Initial position of AllocOp
//   \   /
//    bb2
< ASCII >
// BufferHoisting expected behavior: It should move the existing AllocOp to
// the entry block.
```
## Visual type:
- #program-graph


== ./llvm/llvm_166.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-hoisting.mlir#L146-L152

```c
< ASCII >
// Test Case:
//    bb0 <- Initial position of the first AllocOp
//   /   \
//  bb1  bb2
//   \   /
//    bb3 <- Initial position of the second AllocOp
< ASCII >
// BufferHoisting expected behavior: It shouldn't move the AllocOps.
```
## Visual type:
- #program-graph


== ./llvm/llvm_167.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-hoisting.mlir#L184-L190

```c
// Test Case: No users for buffer in if-else CFG
< ASCII >
//    bb0 <- Initial position of AllocOp
//   /   \
//  bb1  bb2
//   \   /
//    bb3
< ASCII >
// BufferHoisting expected behavior: It shouldn't move the AllocOp.
```
## Visual type:
- #program-graph


== ./llvm/llvm_168.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-hoisting.mlir#L213-L222

```c
< ASCII >
// Test Case:
//      bb0 <- Initial position of the first AllocOp
//     /    \
//   bb1    bb2
//    |     /  \
//    |   bb3  bb4
//    \     \  /
//     \     /
//       bb5 <- Initial position of the second AllocOp
< ASCII >
// BufferHoisting expected behavior: AllocOps shouldn't be moved.
```
## Visual type:
- #program-graph


== ./llvm/llvm_169.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-hoisting.mlir#L276-L283

```c
// Test Case:
< ASCII >
//                                     bb0
//                                    /   \
// Initial pos of the 1st AllocOp -> bb1  bb2 <- Initial pos of the 2nd AllocOp
//                                    \   /
//                                     bb3
< ASCII >
// BufferHoisting expected behavior: Both AllocOps should be moved to the
// entry block.
```
## Visual type:
- #program-graph


== ./llvm/llvm_17.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libcxxabi/test/dynamic_cast3.pass.cpp#L1443-L1451

```c
/*

< ASCII >
A1 A1
|  |
A2 |
 \ |
  A3
< ASCII >

*/
```
## Visual type:
- #tree


== ./llvm/llvm_170.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-hoisting.mlir#L310-L318

```c
// Test Case: Invalid position of the DeallocOp. There is a user after
// deallocation.
< ASCII >
//   bb0
//  /   \
// bb1  bb2 <- Initial position of AllocOp
//  \   /
//   bb3
< ASCII >
// BufferHoisting expected behavior: It should move the AllocOp to the entry
// block.
```
## Visual type:
- #program-graph


== ./llvm/llvm_171.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-loop-hoisting.mlir#L6-L12

```c
< ASCII >
// Test Case:
//    bb0
//   /   \
//  bb1  bb2 <- Initial position of AllocOp
//   \   /
//    bb3
< ASCII >
// BufferLoopHoisting expected behavior: It should not move the AllocOp.
```
## Visual type:
- #program-graph


== ./llvm/llvm_172.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Dialect/Bufferization/Transforms/buffer-loop-hoisting.mlir#L33-L41

```c
< ASCII >
// Test Case:
//    bb0
//   /   \
//  bb1  bb2 <- Initial position of AllocOp
//   \   /
//    bb3
< ASCII >
// BufferLoopHoisting expected behavior: It should not move the existing AllocOp
// to any other block since the alloc has a dynamic dependency to block argument
// %0 in bb2.
```
## Visual type:
- #program-graph


== ./llvm/llvm_173.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Integration/Dialect/Vector/CPU/test-sparse-dot-matvec.mlir#L6-L44

```c
// Illustrates an 8x8 Sparse Matrix x Vector implemented with only operations
// of the vector dialect (and some std/scf). Essentially, this example performs
// the following multiplication:
//
< ASCII >
//     0  1  2  3  4  5  6  7
//   +------------------------+
// 0 | 1  0  2  0  0  1  0  1 |   | 1 |   | 21 |
// 1 | 1  8  0  0  3  0  1  0 |   | 2 |   | 39 |
// 2 | 0  0  1  0  0  2  6  2 |   | 3 |   | 73 |
// 3 | 0  3  0  1  0  1  0  1 | x | 4 | = | 24 |
// 4 | 5  0  0  1  1  1  0  0 |   | 5 |   | 20 |
// 5 | 0  3  0  0  2  1  2  0 |   | 6 |   | 36 |
// 6 | 4  0  7  0  1  0  1  0 |   | 7 |   | 37 |
// 7 | 0  3  0  2  0  0  1  1 |   | 8 |   | 29 |
< ASCII >
//   +------------------------+
//
// The sparse storage scheme used is an extended column scheme (also referred
// to as jagged diagonal, which is essentially a vector friendly variant of
// the general sparse row-wise scheme (also called compressed row storage),
// using fixed length vectors and no explicit pointer indexing into the
// value array to find the rows.
//
< ASCII >
// The extended column storage for the matrix shown above is as follows.
//
//      VALUE           INDEX
//   +---------+     +---------+
// 0 | 1 2 1 1 |     | 0 2 5 7 |
// 1 | 1 8 3 1 |     | 0 1 4 6 |
// 2 | 1 2 6 2 |     | 2 5 6 7 |
// 3 | 3 1 1 1 |     | 1 3 5 7 |
// 4 | 5 1 1 1 |     | 0 3 4 5 |
// 5 | 3 2 1 2 |     | 1 4 5 6 |
// 6 | 4 7 1 1 |     | 0 2 4 6 |
// 7 | 3 2 1 1 |     | 1 3 6 7 |
//   +---------+     +---------+
< ASCII >
//
// This example illustrates a DOT version for the operation. Another example
// in this directory illustrates an effective SAXPY version that operates on the
// transposed jagged diagonal storage to obtain higher vector lengths.
```
## Visual type:
- #matrix


== ./llvm/llvm_174.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Integration/Dialect/Vector/CPU/test-sparse-saxpy-jagged-matvec.mlir#L6-L45

```c
// Illustrates an 8x8 Sparse Matrix x Vector implemented with only operations
// of the vector dialect (and some std/scf). Essentially, this example performs
// the following multiplication:
//
< ASCII >
//     0  1  2  3  4  5  6  7
//   +------------------------+
// 0 | 1  0  2  0  0  1  0  1 |   | 1 |   | 21 |
// 1 | 1  8  0  0  3  0  1  0 |   | 2 |   | 39 |
// 2 | 0  0  1  0  0  2  6  2 |   | 3 |   | 73 |
// 3 | 0  3  0  1  0  1  0  1 | x | 4 | = | 24 |
// 4 | 5  0  0  1  1  1  0  0 |   | 5 |   | 20 |
// 5 | 0  3  0  0  2  1  2  0 |   | 6 |   | 36 |
// 6 | 4  0  7  0  1  0  1  0 |   | 7 |   | 37 |
// 7 | 0  3  0  2  0  0  1  1 |   | 8 |   | 29 |
//   +------------------------+
< ASCII >
//
// The sparse storage scheme used is an extended column scheme (also referred
// to as jagged diagonal, which is essentially a vector friendly variant of
// the general sparse row-wise scheme (also called compressed row storage),
// using fixed length vectors and no explicit pointer indexing into the
// value array to find the rows.
//
// The extended column storage for the matrix shown above is as follows.
//
< ASCII >
//      VALUE           INDEX
//   +---------+     +---------+
// 0 | 1 2 1 1 |     | 0 2 5 7 |
// 1 | 1 8 3 1 |     | 0 1 4 6 |
// 2 | 1 2 6 2 |     | 2 5 6 7 |
// 3 | 3 1 1 1 |     | 1 3 5 7 |
// 4 | 5 1 1 1 |     | 0 3 4 5 |
// 5 | 3 2 1 2 |     | 1 4 5 6 |
// 6 | 4 7 1 1 |     | 0 2 4 6 |
// 7 | 3 2 1 1 |     | 1 3 6 7 |
//   +---------+     +---------+
< ASCII >
//
// This example illustrates an effective SAXPY version that operates
// on the transposed jagged diagonal storage to obtain higher vector
// lengths. Another example in this directory illustrates a DOT
// version of the operation.
```
## Visual type:
- #matrix


== ./llvm/llvm_175.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Transforms/promote-buffers-to-stack.mlir#L8-L15

```c
// Test Case:
< ASCII >
//    bb0
//   /   \
//  bb1  bb2 <- Initial position of AllocOp
//   \   /
//    bb3
< ASCII >
// PromoteBuffersToStack expected behavior: It should convert %0 into an
// AllocaOp.
```
## Visual type:
- #program-graph


== ./llvm/llvm_176.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Transforms/promote-buffers-to-stack.mlir#L39-L46

```c
< ASCII >
// Test Case:
//    bb0
//   /   \
//  bb1  bb2 <- Initial position of AllocOp
//   \   /
//    bb3
< ASCII >
// PromoteBuffersToStack expected behavior:
// Since the alloc has dynamic type, it is not converted into an alloca.
```
## Visual type:
- #program-graph


== ./llvm/llvm_177.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Transforms/promote-buffers-to-stack.mlir#L127-L134

```c
< ASCII >
// Test Case:
//    bb0
//   /   \
//  |    bb1 <- Initial position of AllocOp
//   \   /
//    bb2
< ASCII >
// PromoteBuffersToStack expected behavior: It should convert it into an
// AllocaOp.
```
## Visual type:
- #program-graph


== ./llvm/llvm_178.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Transforms/promote-buffers-to-stack.mlir#L156-L162

```c
< ASCII >
// Test Case:
//    bb0 <- Initial position of AllocOp
//   /   \
//  |    bb1
//   \   /
//    bb2
< ASCII >
// PromoteBuffersToStack expected behavior: It converts the alloc in an alloca.
```
## Visual type:
- #program-graph


== ./llvm/llvm_179.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Transforms/promote-buffers-to-stack.mlir#L183-L189

```c
< ASCII >
// Test Case:
//    bb0 <- Initial position of the first AllocOp
//   /   \
//  bb1  bb2
//   \   /
//    bb3 <- Initial position of the second AllocOp
< ASCII >
// PromoteBuffersToStack expected behavior: It converts the allocs into allocas.
```
## Visual type:
- #program-graph


== ./llvm/llvm_18.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L52-L53

```c
< ASCII >
// ADRP
// | 1 | immlo (2) | 1 | 0 0 0 0 | immhi (19) | Rd (5) |
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_180.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Transforms/promote-buffers-to-stack.mlir#L218-L224

```c
// Test Case: No users for buffer in if-else CFG
< ASCII >
//    bb0 <- Initial position of AllocOp
//   /   \
//  bb1  bb2
//   \   /
//    bb3
< ASCII >
// PromoteBuffersToStack expected behavior: It converts the alloc into alloca.
```
## Visual type:
- #program-graph


== ./llvm/llvm_181.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Transforms/promote-buffers-to-stack.mlir#L247-L257

```c
< ASCII >
// Test Case:
//      bb0 <- Initial position of the first AllocOp
//     /    \
//   bb1    bb2
//    |     /  \
//    |   bb3  bb4
//    \     \  /
//     \     /
//       bb5 <- Initial position of the second AllocOp
< ASCII >
// PromoteBuffersToStack expected behavior: The two allocs should be converted
// into allocas.
```
## Visual type:
- #program-graph


== ./llvm/llvm_182.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/mlir/test/Transforms/promote-buffers-to-stack.mlir#L312-L319

```c
< ASCII >
// Test Case:
//                                     bb0
//                                    /   \
// Initial pos of the 1st AllocOp -> bb1  bb2 <- Initial pos of the 2nd AllocOp
//                                    \   /
//                                     bb3
< ASCII >
// PromoteBuffersToStack expected behavior: Both AllocOps are converted into
// allocas.
```
## Visual type:
- #program-graph


== ./llvm/llvm_183.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/openmp/runtime/src/kmp_lock.cpp#L969-L1019

```c
< ASCII >
/* First the states
   (head,tail) =              0, 0  means lock is unheld, nobody on queue
                 UINT_MAX or -1, 0  means lock is held, nobody on queue
                              h, h  means lock held or about to transition,
                                    1 element on queue
                              h, t  h <> t, means lock is held or about to
                                    transition, >1 elements on queue

   Now the transitions
      Acquire(0,0)  = -1 ,0
      Release(0,0)  = Error
      Acquire(-1,0) =  h ,h    h > 0
      Release(-1,0) =  0 ,0
      Acquire(h,h)  =  h ,t    h > 0, t > 0, h <> t
      Release(h,h)  = -1 ,0    h > 0
      Acquire(h,t)  =  h ,t'   h > 0, t > 0, t' > 0, h <> t, h <> t', t <> t'
      Release(h,t)  =  h',t    h > 0, t > 0, h <> t, h <> h', h' maybe = t

   And pictorially

           +-----+
           | 0, 0|------- release -------> Error
           +-----+
             |  ^
      acquire|  |release
             |  |
             |  |
             v  |
           +-----+
           |-1, 0|
           +-----+
             |  ^
      acquire|  |release
             |  |
             |  |
             v  |
           +-----+
           | h, h|
           +-----+
             |  ^
      acquire|  |release
             |  |
             |  |
             v  |
           +-----+
           | h, t|----- acquire, release loopback ---+
           +-----+                                   |
                ^                                    |
                |                                    |
                +------------------------------------+
< ASCII >
 */
```
## Visual type:
- #state-machine


== ./llvm/llvm_184.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/openmp/runtime/src/kmp_stats.h#L582-L614

```c
/* ****************************************************************
    Class to implement an event

    There are four components to an event: start time, stop time
    nest_level, and timer_name.
    The start and stop time should be obvious (recorded in clock ticks).
    The nest_level relates to the bar width in the timeline graph.
    The timer_name is used to determine which timer event triggered this event.

    the interface to this class is through four read-only operations:
    1) getStart()     -- returns the start time as 64 bit integer
    2) getStop()      -- returns the stop time as 64 bit integer
    3) getNestLevel() -- returns the nest level of the event
    4) getTimerName() -- returns the timer name that triggered event

    *MORE ON NEST_LEVEL*
    The nest level is used in the bar graph that represents the timeline.
    Its main purpose is for showing how events are nested inside eachother.
    For example, say events, A, B, and C are recorded.  If the timeline
    looks like this:

< ASCII >
Begin -------------------------------------------------------------> Time
         |    |          |        |          |              |
         A    B          C        C          B              A
       start start     start     end        end            end
< ASCII >

       Then A, B, C will have a nest level of 1, 2, 3 respectively.
       These values are then used to calculate the barwidth so you can
       see that inside A, B has occurred, and inside B, C has occurred.
       Currently, this is shown with A's bar width being larger than B's
       bar width, and B's bar width being larger than C's bar width.

**************************************************************** */
```
## Visual type:
- #interval


== ./llvm/llvm_185.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/openmp/runtime/src/kmp_stats.h#L699-L725

```c
/* ****************************************************************
    Class to implement a doubly-linked, circular, statistics list

< ASCII >
    |---| ---> |---| ---> |---| ---> |---| ---> ... next
    |     |  |  |  |  |  |  |
    | --- |<--- |---| <--- |---| <--- |---| <--- ... prev
    Sentinel   first      second     third
    Node       node       node       node
< ASCII >

    The Sentinel Node is the user handle on the list.
    The first node corresponds to thread 0's statistics.
    The second node corresponds to thread 1's statistics and so on...

    Each node has a _timers, _counters, and _explicitTimers array to hold that
    thread's statistics. The _explicitTimers point to the correct _timer and
    update its statistics at every stop() call. The explicitTimers' pointers are
    set up in the constructor. Each node also has an event vector to hold that
    thread's timing events. The event vector expands as necessary and records
    the start-stop times for each timer.

    The nestLevel variable is for plotting events and is related
    to the bar width in the timeline graph.

    Every thread will have a thread local pointer to its node in
    the list.  The sentinel node is used by the primary thread to
    store "dummy" statistics before __kmp_create_worker() is called.
**************************************************************** */
```
## Visual type:
- #custom


== ./llvm/llvm_186.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/openmp/runtime/test/tasking/kmp_task_depend_all.c#L5-L22

```c
// Tests OMP 5.x task dependence "omp_all_memory",
// emulates compiler codegen versions for new dep kind
//
< ASCII >
// Task tree created:
//      task0 - task1 (in: i1, i2)
//             \
//        task2 (inoutset: i2), (in: i1)
//             /
//        task3 (omp_all_memory) via flag=0x80
//             /
//      task4 - task5 (in: i1, i2)
//           /
//       task6 (omp_all_memory) via addr=-1
//           /
//       task7 (omp_all_memory) via flag=0x80
//           /
//       task8 (in: i3)
< ASCII >
//
```
## Visual type:
- #tree


== ./llvm/llvm_187.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/openmp/runtime/test/tasking/omp50_task_depend_mtx.c#L3-L16

```c
// Tests OMP 5.0 task dependences "mutexinoutset", emulates compiler codegen
// Mutually exclusive tasks get same input dependency info array
//
< ASCII >
// Task tree created:
//      task0 task1
//         \    / \
//         task2   task5
//           / \
//       task3  task4
//       /   \
//  task6 <-->task7  (these two are mutually exclusive)
//       \    /
//       task8
< ASCII >
//
```
## Visual type:
- #tree


== ./llvm/llvm_188.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/openmp/runtime/test/tasking/omp50_task_depend_mtx2.c#L3-L16

```c
// Tests OMP 5.0 task dependences "mutexinoutset", emulates compiler codegen
// Mutually exclusive tasks get input dependency info array sorted differently
//
< ASCII >
// Task tree created:
//      task0 task1
//         \    / \
//         task2   task5
//           / \
//       task3  task4
//       /   \
//  task6 <-->task7  (these two are mutually exclusive)
//       \    /
//       task8
< ASCII >
//
```
## Visual type:
- #tree


== ./llvm/llvm_189.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/openmp/runtime/test/tasking/omp50_task_depend_mtx3.c#L7-L20

```c
// Tests OMP 5.0 task dependences "mutexinoutset", emulates compiler codegen
// Mutually exclusive tasks get same input dependency info array
//
< ASCII >
// Task tree created:
//      task0 task1
//         \    / \
//         task2   task5
//           / \
//       task3  task4
//       /   \
//  task6 <-->task7  (these two are mutually exclusive)
//       \    /
//       task8
< ASCII >
//
```
## Visual type:
- #tree


== ./llvm/llvm_19.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L62-L63

```c
< ASCII >
// All loads and stores have 1 (at bit position 27), (0 at bit position 25).
// | op0 x op1 (2) | 1 op2 0 op3 (2) | x | op4 (5) | xxxx | op5 (2) | x (10) |
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_190.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/openmp/runtime/test/tasking/omp51_task_dep_inoutset.c#L5-L19

```c
// Tests OMP 5.0 task dependences "mutexinoutset" and 5.1 "inoutset",
// emulates compiler codegen for new dep kinds
// Mutually exclusive tasks get same input dependency info array
//
< ASCII >
// Task tree created:
//      task0 - task1 (in)
//             \
//        task2 - task3 (inoutset)
//             /
//      task3 - task4 (in)
//           /
//  task6 <-->task7  (mutexinoutset)
//       \    /
//       task8 (in)
< ASCII >
//
```
## Visual type:
- #tree


== ./llvm/llvm_191.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/openmp/runtime/test/worksharing/for/omp_for_schedule_guided.c#L3-L18

```c
/* Test for guided scheduling
 * Ensure threads get chunks interleavely first
 * Then judge the chunk sizes are decreasing to a stable value
 * Modified by Chunhua Liao
 * For example, 100 iteration on 2 threads, chunksize 7
 * one line for each dispatch, 0/1 means thread id
< ASCII >
 * 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0  24
 * 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1        18
 * 0 0 0 0 0 0 0 0 0 0 0 0 0 0            14
 * 1 1 1 1 1 1 1 1 1 1                10
 * 0 0 0 0 0 0 0 0                   8
 * 1 1 1 1 1 1 1                   7
 * 0 0 0 0 0 0 0                   7
 * 1 1 1 1 1 1 1                   7
 * 0 0 0 0 0                     5
< ASCII >
*/
```
## Visual type:
- #custom


== ./llvm/llvm_192.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/include/polly/CodeGen/Utils.h#L32-L67

```c
/// Execute a Scop conditionally wrt @p RTC.
///
/// In the CFG the optimized code of the Scop is generated next to the
/// original code. Both the new and the original version of the code remain
/// in the CFG. A branch statement decides which version is executed based on
/// the runtime value of @p RTC.
///
< ASCII >
/// Before transformation:
///
///                        bb0
///                         |
///                     orig_scop
///                         |
///                        bb1
///
/// After transformation:
///                        bb0
///                         |
///                  polly.splitBlock
///                     /       \.
///                     |     startBlock
///                     |        |
///               orig_scop   new_scop
///                     \      /
///                      \    /
///                        bb1 (joinBlock)
< ASCII >
///
/// @param S   The Scop to execute conditionally.
/// @param P   A reference to the pass calling this function.
/// @param RTC The runtime condition checked before executing the new SCoP.
///
/// @return  An std::pair:
///              - The first element is a BBPair of (StartBlock, EndBlock).
///              - The second element is the BranchInst which conditionally
///                branches to the SCoP based on the RTC.
///
```
## Visual type:
- #program-graph


== ./llvm/llvm_193.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/include/polly/Support/GICHelper.h#L25-L53

```c
/// Translate an llvm::APInt to an isl_val.
///
/// Translate the bitsequence without sign information as provided by APInt into
/// a signed isl_val type. Depending on the value of @p IsSigned @p Int is
/// interpreted as unsigned value or as signed value in two's complement
/// representation.
///
< ASCII >
/// Input IsSigned                 Output
///
///     0        0           ->    0
///     1        0           ->    1
///    00        0           ->    0
///    01        0           ->    1
///    10        0           ->    2
///    11        0           ->    3
///
///     0        1           ->    0
///     1        1           ->   -1
///    00        1           ->    0
///    01        1           ->    1
///    10        1           ->   -2
///    11        1           ->   -1
< ASCII >
///
/// @param Ctx      The isl_ctx to create the isl_val in.
/// @param Int      The integer value to translate.
/// @param IsSigned If the APInt should be interpreted as signed or unsigned
///                 value.
///
/// @return The isl_val corresponding to @p Int.
```
## Visual type:
- #table


== ./llvm/llvm_194.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/include/polly/Support/GICHelper.h#L57-L85

```c
/// Translate an llvm::APInt to an isl::val.
///
/// Translate the bitsequence without sign information as provided by APInt into
/// a signed isl::val type. Depending on the value of @p IsSigned @p Int is
/// interpreted as unsigned value or as signed value in two's complement
/// representation.
///
< ASCII >
/// Input IsSigned                 Output
///
///     0        0           ->    0
///     1        0           ->    1
///    00        0           ->    0
///    01        0           ->    1
///    10        0           ->    2
///    11        0           ->    3
///
///     0        1           ->    0
///     1        1           ->   -1
///    00        1           ->    0
///    01        1           ->    1
///    10        1           ->   -2
///    11        1           ->   -1
< ASCII >
///
/// @param Ctx      The isl_ctx to create the isl::val in.
/// @param Int      The integer value to translate.
/// @param IsSigned If the APInt should be interpreted as signed or unsigned
///                 value.
///
/// @return The isl::val corresponding to @p Int.
```
## Visual type:
- #table


== ./llvm/llvm_195.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/include/polly/Support/GICHelper.h#L91-L117

```c
/// Translate isl_val to llvm::APInt.
///
/// This function can only be called on isl_val values which are integers.
/// Calling this function with a non-integral rational, NaN or infinity value
/// is not allowed.
///
/// As the input isl_val may be negative, the APInt that this function returns
/// must always be interpreted as signed two's complement value. The bitwidth of
/// the generated APInt is always the minimal bitwidth necessary to model the
/// provided integer when interpreting the bit pattern as signed value.
///
< ASCII >
/// Some example conversions are:
///
///   Input      Bits    Signed  Bitwidth
///       0 ->      0         0         1
///      -1 ->      1        -1         1
///       1 ->     01         1         2
///      -2 ->     10        -2         2
///       2 ->    010         2         3
///      -3 ->    101        -3         3
///       3 ->    011         3         3
///      -4 ->    100        -4         3
///       4 ->   0100         4         4
< ASCII >
///
/// @param Val The isl val to translate.
///
/// @return The APInt value corresponding to @p Val.
```
## Visual type:
- #table


== ./llvm/llvm_196.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/include/polly/Support/GICHelper.h#L120-L146

```c
/// Translate isl::val to llvm::APInt.
///
/// This function can only be called on isl::val values which are integers.
/// Calling this function with a non-integral rational, NaN or infinity value
/// is not allowed.
///
/// As the input isl::val may be negative, the APInt that this function returns
/// must always be interpreted as signed two's complement value. The bitwidth of
/// the generated APInt is always the minimal bitwidth necessary to model the
/// provided integer when interpreting the bit pattern as signed value.
///
< ASCII >
/// Some example conversions are:
///
///   Input      Bits    Signed  Bitwidth
///       0 ->      0         0         1
///      -1 ->      1        -1         1
///       1 ->     01         1         2
///      -2 ->     10        -2         2
///       2 ->    010         2         3
///      -3 ->    101        -3         3
///       3 ->    011         3         3
///      -4 ->    100        -4         3
///       4 ->   0100         4         4
< ASCII >
///
/// @param Val The isl val to translate.
///
/// @return The APInt value corresponding to @p Val.
```
## Visual type:
- #table


== ./llvm/llvm_197.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/CodeGen/LoopGenerators.cpp#L59-L82

```c
// We generate a loop of either of the following structures:
//
< ASCII >
//              BeforeBB                      BeforeBB
//                 |                             |
//                 v                             v
//              GuardBB                      PreHeaderBB
//              /      |                         |   _____
//     __  PreHeaderBB  |                        v  \/    |
//    /  \    /         |                     HeaderBB  latch
// latch  HeaderBB      |                        |\       |
//    \  /    \         /                        | \------/
//     <       \       /                         |
//              \     /                          v
//              ExitBB                         ExitBB
< ASCII >
//
// depending on whether or not we know that it is executed at least once. If
// not, GuardBB checks if the loop is executed at least once. If this is the
// case we branch to PreHeaderBB and subsequently to the HeaderBB, which
// contains the loop iv 'polly.indvar', the incremented loop iv
// 'polly.indvar_next' as well as the condition to check if we execute another
// iteration of the loop. After the loop has finished, we branch to ExitBB.
// We expect the type of UB, LB, UB+Stride to be large enough for values that
// UB may take throughout the execution of the loop, including the computation
// of indvar + Stride before the final abort.
```
## Visual type:
- #program-graph


== ./llvm/llvm_198.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/CodeGen/LoopGeneratorsGOMP.cpp#L72-L91

```c
// Create a subfunction of the following (preliminary) structure:
//
< ASCII >
//    PrevBB
//       |
//       v
//    HeaderBB
//       |   _____
//       v  v    |
//   CheckNextBB  PreHeaderBB
//       |\       |
//       | \______/
//       |
//       v
//     ExitBB
< ASCII >
//
// HeaderBB will hold allocations and loading of variables.
// CheckNextBB will check for more work.
// If there is more work to do: go to PreHeaderBB, otherwise go to ExitBB.
// PreHeaderBB loads the new boundaries (& will lead to the loop body later on).
// ExitBB marks the end of the parallel execution.
```
## Visual type:
- #program-graph


== ./llvm/llvm_199.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/CodeGen/LoopGeneratorsKMP.cpp#L106-L130

```c
// Create a subfunction of the following (preliminary) structure:
//
< ASCII >
//        PrevBB
//           |
//           v
//        HeaderBB
//       /   |    _____
//      /    v   v     |
//     / PreHeaderBB   |
//    |      |         |
//    |      v         |
//    |  CheckNextBB   |
//     \     |   \_____/
//      \    |
//       v   v
//       ExitBB
< ASCII >
//
// HeaderBB will hold allocations, loading of variables and kmp-init calls.
// CheckNextBB will check for more work (dynamic / static chunked) or will be
// empty (static non chunked).
// If there is more work to do: go to PreHeaderBB, otherwise go to ExitBB.
// PreHeaderBB loads the new boundaries (& will lead to the loop body later on).
// Just like CheckNextBB: PreHeaderBB is (preliminary) empty in the static non
// chunked scheduling case. ExitBB marks the end of the parallel execution.
// The possibly empty BasicBlocks will automatically be removed.
```
## Visual type:
- #program-graph


== ./llvm/llvm_2.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/flang/lib/Semantics/definable.cpp#L63-L72

```c
// When a DataRef contains pointers, gets the rightmost one (unless it is
// the entity being defined, in which case the last pointer above it);
// otherwise, returns the leftmost symbol.  The resulting symbol is the
// relevant base object for definabiliy checking.  Examples:
< ASCII >
//   ptr1%ptr2        => ...     -> ptr1
//   nonptr%ptr       => ...     -> nonptr
//   nonptr%ptr       =  ...     -> ptr
//   ptr1%ptr2        =  ...     -> ptr2
//   ptr1%ptr2%nonptr =  ...     -> ptr2
//   nonptr1%nonptr2  =  ...     -> nonptr1
< ASCII >
```
## Visual type:
- #custom


== ./llvm/llvm_20.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L68-L72

```c
< ASCII >
// LDN/STN multiple no offset
// | 0 Q 00 | 1100 | 0 L 00 | 0000 | opcode (4) | size (2) | Rn (5) | Rt (5) |
// LDN/STN multiple post-indexed
// | 0 Q 00 | 1100 | 1 L 0 | Rm (5)| opcode (4) | size (2) | Rn (5) | Rt (5) |
// L == 0 for stores.
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_200.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/CodeGen/ManagedMemoryRewrite.cpp#L82-L103

```c
// Expand a constant expression `Cur`, which is used at instruction `Parent`
// at index `index`.
// Since a constant expression can expand to multiple instructions, store all
// the expands into a set called `Expands`.
// Note that this goes inorder on the constant expression tree.
// A * ((B * D) + C)
// will be processed with first A, then B * D, then B, then D, and then C.
// Though ConstantExprs are not treated as "trees" but as DAGs, since you can
// have something like this:
< ASCII >
//    *
//   /  \
//   \  /
//    (D)
< ASCII >
//
// For the purposes of this expansion, we expand the two occurences of D
// separately. Therefore, we expand the DAG into the tree:
< ASCII >
//  *
// / \
// D  D
< ASCII >
// TODO: We don't _have_to do this, but this is the simplest solution.
// We can write a solution that keeps track of which constants have been
// already expanded.
```
## Visual type:
- #program-graph


== ./llvm/llvm_201.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_coalesce.c#L709-L759

```c
/* Basic map "i" has an inequality "k" that is adjacent
 * to some inequality of basic map "j".  All the other inequalities
 * are valid for "j".
 * If not NULL, then "extra" contains extra wrapping constraints that are valid
 * for both "i" and "j".
 * Check if basic map "j" forms an extension of basic map "i",
 * taking into account the extra constraints, if any.
 *
 * Note that this function is only called if some of the equalities or
 * inequalities of basic map "j" do cut basic map "i".  The function is
 * correct even if there are no such cut constraints, but in that case
 * the additional checks performed by this function are overkill.
 *
 * In particular, we replace constraint k, say f >= 0, by constraint
 * f <= -1, add the inequalities of "j" that are valid for "i",
 * as well as the "extra" constraints, if any,
 * and check if the result is a subset of basic map "j".
 * To improve the chances of the subset relation being detected,
 * any variable that only attains a single integer value
 * in the tableau of "i" is first fixed to that value.
 * If the result is a subset, then we know that this result is exactly equal
 * to basic map "j" since all its constraints are valid for basic map "j".
 * By combining the valid constraints of "i" (all equalities and all
 * inequalities except "k"), the valid constraints of "j" and
 * the "extra" constraints, if any, we therefore
 * obtain a basic map that is equal to their union.
 * In this case, there is no need to perform a rollback of the tableau
 * since it is going to be destroyed in fuse().
 *
< ASCII >
 *
 *	|\__			|\__
 *	|   \__			|   \__
 *	|      \_	=>	|      \__
 *	|_______| _		|_________\
 *
 *
 *	|\			|\
 *	| \			| \
 *	|  \			|  \
 *	|  |			|   \
 *	|  ||\		=>      |    \
 *	|  || \			|     \
 *	|  ||  |		|      |
 *	|__||_/			|_____/
 *
 *
 *	_______			 _______
 *     |       | __		|       \__
 *     |       ||__|	=>	|        __|
 *     |_______|		|_______/
< ASCII >
 */
```
## Visual type:
- #custom


== ./llvm/llvm_202.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_coalesce.c#L1073-L1116

```c
/* Basic map "i" has "n" inequality constraints (collected in "relax")
 * that are such that they include basic map "j" if they are relaxed
 * by one.  All the other inequalities are valid for "j".
 * Check if basic map "j" forms an extension of basic map "i".
 *
 * In particular, relax the constraints in "relax", compute the corresponding
 * facets one by one and check whether each of these is included
 * in the other basic map.
 * Before testing for inclusion, the constraints on each facet
 * are tightened to increase the chance of an inclusion being detected.
 * (Adding the valid constraints of "j" to the tableau of "i", as is done
 * in is_adj_ineq_extension, may further increase those chances, but this
 * is not currently done.)
 * If each facet is included, we know that relaxing the constraints extends
 * the basic map with exactly the other basic map (we already know that this
 * other basic map is included in the extension, because all other
 * inequality constraints are valid of "j") and we can replace the
 * two basic maps by this extension.
 *
 * If any of the relaxed constraints turn out to be redundant, then bail out.
 * isl_tab_select_facet refuses to handle such constraints.  It may be
 * possible to handle them anyway by making a distinction between
 * redundant constraints with a corresponding facet that still intersects
 * the set (allowing isl_tab_select_facet to handle them) and
 * those where the facet does not intersect the set (which can be ignored
 * because the empty facet is trivially included in the other disjunct).
 * However, relaxed constraints that turn out to be redundant should
 * be fairly rare and no such instance has been reported where
 * coalescing would be successful.
< ASCII >
 *        ____			  _____
 *       /    || 		 /     |
 *      /     ||  		/      |
 *      \     ||   	=>	\      |
 *       \    ||		 \     |
 *        \___||		  \____|
 *
 *
 *	 \			|\
 *	|\\			| \
 *	| \\			|  \
 *	|  |		=>	|  /
 *	| /			| /
 *	|/			|/
< ASCII >
 */
```
## Visual type:
- #custom


== ./llvm/llvm_203.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_coalesce.c#L1679-L1709

```c
/* Both basic maps have at least one inequality with and adjacent
 * (but opposite) inequality in the other basic map.
 * Check that there are no cut constraints and that there is only
 * a single pair of adjacent inequalities.
 * If so, we can replace the pair by a single basic map described
 * by all but the pair of adjacent inequalities.
 * Any additional points introduced lie strictly between the two
 * adjacent hyperplanes and can therefore be integral.
 *
< ASCII >
 *        ____			  _____
 *       /    ||\		 /     \
 *      /     || \		/       \
 *      \     ||  \	=>	\        \
 *       \    ||  /		 \       /
 *        \___||_/		  \_____/
< ASCII >
 *
 * The test for a single pair of adjacent inequalities is important
 * for avoiding the combination of two basic maps like the following
 *
< ASCII >
 *       /|
 *      / |
 *     /__|
 *         _____
 *         |   |
 *         |   |
 *         |___|
< ASCII >
 *
 * If there are some cut constraints on one side, then we may
 * still be able to fuse the two basic maps, but we need to perform
 * some additional checks in is_adj_ineq_extension.
 */
```
## Visual type:
- #custom


== ./llvm/llvm_204.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_coalesce.c#L1737-L1757

```c
/* Given a basic set i with a constraint k that is adjacent to
 * basic set j, check if we can wrap
 * both the facet corresponding to k (if "wrap_facet" is set) and basic map j
 * (always) around their ridges to include the other set.
 * If so, replace the pair of basic sets by their union.
 *
 * All constraints of i (except k) are assumed to be valid or
 * cut constraints for j.
 * Wrapping the cut constraints to include basic map j may result
 * in constraints that are no longer valid of basic map i
 * we have to check that the resulting wrapping constraints are valid for i.
 * If "wrap_facet" is not set, then all constraints of i (except k)
 * are assumed to be valid for j.
< ASCII >
 *        ____			  _____
 *       /    | 		 /     \
 *      /     ||  		/      |
 *      \     ||   	=>	\      |
 *       \    ||		 \     |
 *        \___||		  \____|
< ASCII >
 *
 */
```
## Visual type:
- #custom


== ./llvm/llvm_205.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_coalesce.c#L1995-L2053

```c
/* Given two basic sets i and j,
 * check if relaxing all the cut constraints of i by one turns
 * them into valid constraint for j and check if we can wrap in
 * the bits that are sticking out.
 * If so, replace the pair by their union.
 *
 * We first check if all relaxed cut inequalities of i are valid for j
 * and then try to wrap in the intersections of the relaxed cut inequalities
 * with j.
 *
 * During this wrapping, we consider the points of j that lie at a distance
 * of exactly 1 from i.  In particular, we ignore the points that lie in
 * between this lower-dimensional space and the basic map i.
 * We can therefore only apply this to integer maps.
< ASCII >
 *        ____			  _____
 *       / ___|_		 /     \
 *      / |    |  		/      |
 *      \ |    |   	=>	\      |
 *       \|____|		 \     |
 *        \___| 		  \____/
 *
 *	 _____			 ______
 *	| ____|_		|      \
 *	| |     |		|       |
 *	| |	|	=>	|       |
 *	|_|     |		|       |
 *	  |_____|		 \______|
 *
 *	 _______
 *	|       |
 *	|  |\   |
 *	|  | \  |
 *	|  |  \ |
 *	|  |   \|
 *	|  |    \
 *	|  |_____\
 *	|       |
 *	|_______|
< ASCII >
 *
 * Wrapping can fail if the result of wrapping one of the facets
 * around its edges does not produce any new facet constraint.
 * In particular, this happens when we try to wrap in unbounded sets.
 *
< ASCII >
 *	 _______________________________________________________________________
 *	|
 *	|  ___
 *	| |   |
 *	|_|   |_________________________________________________________________
 *	  |___|
 *
 * The following is not an acceptable result of coalescing the above two
 * sets as it includes extra integer points.
 *	 _______________________________________________________________________
 *	|
 *	|     
 *	|      
 *	|
 *	 \______________________________________________________________________
< ASCII >
 */
```
## Visual type:
- #custom


== ./llvm/llvm_206.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_coalesce.c#L2278-L2295

```c
/* The two basic maps lie on adjacent hyperplanes.  In particular,
 * basic map "i" has an equality that lies parallel to basic map "j".
 * Check if we can wrap the facets around the parallel hyperplanes
 * to include the other set.
 *
 * We perform basically the same operations as can_wrap_in_facet,
 * except that we don't need to select a facet of one of the sets.
< ASCII >
 *				_
 *	\\			\\
 *	 \\		=>	 \\
 *	  \			  \|
< ASCII >
 *
 * If there is more than one equality of "i" adjacent to an equality of "j",
 * then the result will satisfy one or more equalities that are a linear
 * combination of these equalities.  These will be encoded as pairs
 * of inequalities in the wrapping constraints and need to be made
 * explicit.
 */
```
## Visual type:
- #custom


== ./llvm/llvm_207.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_int_sioimath.h#L31-L71

```c
/* The type to represent integers optimized for small values. It is either a
 * pointer to an mp_int ( = mpz_t*; big representation) or an int32_t (small
 * represenation) with a discriminator at the least significant bit. In big
 * representation it will be always zero because of heap alignment. It is set
 * to 1 for small representation and use the 32 most significant bits for the
 * int32_t.
 *
< ASCII >
 * Structure on 64 bit machines, with 8-byte aligment (3 bits):
 *
 * Big representation:
 * MSB                                                          LSB
 * |------------------------------------------------------------000
 * |                            mpz_t*                            |
 * |                           != NULL                            |
 *
 * Small representation:
 * MSB                           32                             LSB
 * |------------------------------|00000000000000000000000000000001
 * |          int32_t             |
 * |  2147483647 ... -2147483647  |
 *                                                                ^
 *                                                                |
 *                                                        discriminator bit
< ASCII >
 *
 * On 32 bit machines isl_sioimath type is blown up to 8 bytes, i.e.
 * isl_sioimath is guaranteed to be at least 8 bytes. This is to ensure the
 * int32_t can be hidden in that type without data loss. In the future we might
 * optimize this to use 31 hidden bits in a 32 bit pointer. We may also use 63
 * bits on 64 bit machines, but this comes with the cost of additional overflow
 * checks because there is no standardized 128 bit integer we could expand to.
 *
 * We use native integer types and avoid union structures to avoid assumptions
 * on the machine's endianness.
 *
 * This implementation makes the following assumptions:
 * - long can represent any int32_t
 * - mp_small is signed long
 * - mp_usmall is unsigned long
 * - adresses returned by malloc are aligned to 2-byte boundaries (leastmost
 *   bit is zero)
 */
```
## Visual type:
- #memory-layout


== ./llvm/llvm_208.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_int_sioimath.h#L164-L185

```c
/* A common situation is to call an IMath function with at least one argument
 * that is currently in small representation or an integer parameter, i.e. a big
 * representation of the same number is required. Promoting the original
 * argument comes with multiple problems, such as modifying a read-only
 * argument, the responsibility of deallocation and the execution cost. Instead,
 * we make a copy by 'faking' the IMath internal structure.
 *
 * We reserve the maximum number of required digits on the stack to avoid heap
 * allocations.
 *
 * mp_digit can be uint32_t or uint16_t. This code must work for little and big
 * endian digits. The structure for an uint64_t argument and 32-bit mp_digits is
 * sketched below.
 *
< ASCII >
 * |----------------------------|
 *            uint64_t
 *
 * |-------------||-------------|
 *    mp_digit        mp_digit
 *    digits[1]       digits[0]
 * Most sig digit  Least sig digit
< ASCII >
 */
```
## Visual type:
- #memory-layout


== ./llvm/llvm_209.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_mat.c#L2039-L2050

```c
/* Return rows that extend a basis of "mat1" to one
 * that covers both "mat1" and "mat2".
 * The Hermite normal form of the concatenation of the two matrices is
 *
< ASCII >
 *	                     [ Q1 ]
 *	[ M1 ] = [ H1 0  0 ] [ Q2 ]
 *	[ M2 ] = [ H2 H3 0 ] [ Q3 ]
< ASCII >
 *
 * The number of columns in H1 and H3 determine the number of rows
 * in Q1 and Q2.  Q1 is a basis for M1, while Q2 extends this basis
 * to also cover M2.
 */
```
## Visual type:
- #formula


== ./llvm/llvm_21.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L96-L100

```c
< ASCII >
// LDN/STN single no offset
// | 0 Q 00 | 1101 | 0 L R 0 | 0000 | opc (3) S | size (2) | Rn (5) | Rt (5)|
// LDN/STN single post-indexed
// | 0 Q 00 | 1101 | 1 L R | Rm (5) | opc (3) S | size (2) | Rn (5) | Rt (5)|
// L == 0 for stores
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_210.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_scheduler.c#L726-L731

```c
< ASCII >
/* Construct a map that isolates the variable in position "pos" in "set".
 *
 * That is, construct
 *
 *	[i_0, ..., i_pos-1, i_pos+1, ...] -> [i_pos]
< ASCII >
 */
```
## Visual type:



== ./llvm/llvm_211.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_scheduler.c#L741-L756

```c
/* Compute and return the size of "set" in dimension "dim".
 * The size is taken to be the difference in values for that variable
 * for fixed values of the other variables.
 * This assumes that "set" is convex.
 * In particular, the variable is first isolated from the other variables
 * in the range of a map
 *
< ASCII >
 *	[i_0, ..., i_dim-1, i_dim+1, ...] -> [i_dim]
 *
 * and then duplicated
 *
 *	[i_0, ..., i_dim-1, i_dim+1, ...] -> [[i_dim] -> [i_dim']]
< ASCII >
 *
 * The shared variables are then projected out and the maximal value
 * of i_dim' - i_dim is computed.
 */
```
## Visual type:



== ./llvm/llvm_212.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_tab_pip.c#L3783-L3838

```c
/* Determine the sign of the given row of the main tableau.
 * The result is one of
 *	isl_tab_row_pos: always non-negative; no pivot needed
 *	isl_tab_row_neg: always non-positive; pivot
 *	isl_tab_row_any: can be both positive and negative; split
 *
 * We first handle some simple cases
 *	- the row sign may be known already
 *	- the row may be obviously non-negative
 *	- the parametric constant may be equal to that of another row
 *	  for which we know the sign.  This sign will be either "pos" or
 *	  "any".  If it had been "neg" then we would have pivoted before.
 *
 * If none of these cases hold, we check the value of the row for each
 * of the currently active samples.  Based on the signs of these values
 * we make an initial determination of the sign of the row.
 *
 *	all zero			->	unk(nown)
 *	all non-negative		->	pos
 *	all non-positive		->	neg
 *	both negative and positive	->	all
 *
 * If we end up with "all", we are done.
 * Otherwise, we perform a check for positive and/or negative
 * values as follows.
 *
< ASCII >
 *	samples	       neg	       unk	       pos
 *	<0 ?			    Y        N	    Y        N
 *					    pos    any      pos
 *	>0 ?	     Y      N	 Y     N
 *		    any    neg  any   neg
< ASCII >
 *
 * There is no special sign for "zero", because we can usually treat zero
 * as either non-negative or non-positive, whatever works out best.
 * However, if the row is "critical", meaning that pivoting is impossible
 * then we don't want to limp zero with the non-positive case, because
 * then we we would lose the solution for those values of the parameters
 * where the value of the row is zero.  Instead, we treat 0 as non-negative
 * ensuring a split if the row can attain both zero and negative values.
 * The same happens when the original constraint was one that could not
 * be satisfied with equality by any integer values of the parameters.
 * In this case, we normalize the constraint, but then a value of zero
 * for the normalized constraint is actually a positive value for the
 * original constraint, so again we need to treat zero as non-negative.
 * In both these cases, we have the following decision tree instead:
 *
 *	all non-negative		->	pos
 *	all negative			->	neg
 *	both negative and non-negative	->	all
 *
< ASCII >
 *	samples	       neg	          	       pos
 *	<0 ?			             	    Y        N
 *					           any      pos
 *	>=0 ?	     Y      N
 *		    any    neg
< ASCII >
 */
```
## Visual type:
- #custom


== ./llvm/llvm_213.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/isl/isl_tab_pip.c#L3998-L4091

```c
/* Compute the lexicographic minimum of the set represented by the main
 * tableau "tab" within the context "sol->context_tab".
 * On entry the sample value of the main tableau is lexicographically
 * less than or equal to this lexicographic minimum.
 * Pivots are performed until a feasible point is found, which is then
 * necessarily equal to the minimum, or until the tableau is found to
 * be infeasible.  Some pivots may need to be performed for only some
 * feasible values of the context tableau.  If so, the context tableau
 * is split into a part where the pivot is needed and a part where it is not.
 *
 * Whenever we enter the main loop, the main tableau is such that no
 * "obvious" pivots need to be performed on it, where "obvious" means
 * that the given row can be seen to be negative without looking at
 * the context tableau.  In particular, for non-parametric problems,
 * no pivots need to be performed on the main tableau.
 * The caller of find_solutions is responsible for making this property
 * hold prior to the first iteration of the loop, while restore_lexmin
 * is called before every other iteration.
 *
 * Inside the main loop, we first examine the signs of the rows of
 * the main tableau within the context of the context tableau.
 * If we find a row that is always non-positive for all values of
 * the parameters satisfying the context tableau and negative for at
 * least one value of the parameters, we perform the appropriate pivot
 * and start over.  An exception is the case where no pivot can be
 * performed on the row.  In this case, we require that the sign of
 * the row is negative for all values of the parameters (rather than just
 * non-positive).  This special case is handled inside row_sign, which
 * will say that the row can have any sign if it determines that it can
 * attain both negative and zero values.
 *
 * If we can't find a row that always requires a pivot, but we can find
 * one or more rows that require a pivot for some values of the parameters
 * (i.e., the row can attain both positive and negative signs), then we split
 * the context tableau into two parts, one where we force the sign to be
 * non-negative and one where we force is to be negative.
 * The non-negative part is handled by a recursive call (through find_in_pos).
 * Upon returning from this call, we continue with the negative part and
 * perform the required pivot.
 *
 * If no such rows can be found, all rows are non-negative and we have
 * found a (rational) feasible point.  If we only wanted a rational point
 * then we are done.
 * Otherwise, we check if all values of the sample point of the tableau
 * are integral for the variables.  If so, we have found the minimal
 * integral point and we are done.
 * If the sample point is not integral, then we need to make a distinction
 * based on whether the constant term is non-integral or the coefficients
 * of the parameters.  Furthermore, in order to decide how to handle
 * the non-integrality, we also need to know whether the coefficients
 * of the other columns in the tableau are integral.  This leads
 * to the following table.  The first two rows do not correspond
 * to a non-integral sample point and are only mentioned for completeness.
 *
< ASCII >
 *	constant	parameters	other
 *
 *	int		int		int	|
 *	int		int		rat	| -> no problem
 *
 *	rat		int		int	  -> fail
 *
 *	rat		int		rat	  -> cut
 *
 *	int		rat		rat	|
 *	rat		rat		rat	| -> parametric cut
 *
 *	int		rat		int	|
 *	rat		rat		int	| -> split context
< ASCII >
 *
 * If the parametric constant is completely integral, then there is nothing
 * to be done.  If the constant term is non-integral, but all the other
 * coefficient are integral, then there is nothing that can be done
 * and the tableau has no integral solution.
 * If, on the other hand, one or more of the other columns have rational
 * coefficients, but the parameter coefficients are all integral, then
 * we can perform a regular (non-parametric) cut.
 * Finally, if there is any parameter coefficient that is non-integral,
 * then we need to involve the context tableau.  There are two cases here.
 * If at least one other column has a rational coefficient, then we
 * can perform a parametric cut in the main tableau by adding a new
 * integer division in the context tableau.
 * If all other columns have integral coefficients, then we need to
 * enforce that the rational combination of parameters (c + \sum a_i y_i)/m
 * is always integral.  We do this by introducing an integer division
 * q = floor((c + \sum a_i y_i)/m) and stipulating that its argument should
 * always be integral in the context tableau, i.e., m q = c + \sum a_i y_i.
 * Since q is expressed in the tableau as
 *	c + \sum a_i y_i - m q >= 0
 *	-c - \sum a_i y_i + m q + m - 1 >= 0
 * it is sufficient to add the inequality
 *	-c - \sum a_i y_i + m q >= 0
 * In the part of the context where this inequality does not hold, the
 * main tableau is marked as being empty.
 */
```
## Visual type:
- #table


== ./llvm/llvm_214.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/ppcg/gpu.c#L5599-L5657

```c
/* Generate CUDA code for "scop" and print it to "p".
 * After generating an AST for the transformed scop as explained below,
 * we call "gen->print" to print the AST in the desired output format
 * to "p".
 *
 * If it turns out that it does not make sense to generate GPU code,
 * then we generate CPU code instead.
 *
 * The declarations of the arrays that are visible outside of the scop
 * are printed outside of the code generated from the schedule,
 * because the generated code may involve a guard around the entire code.
 *
 * We first compute a schedule that respects the dependences
 * of the original program and select the outermost bands
 * of tilable dimensions that have at least one parallel loop.
 * If the --load-schedule is specified, then the loaded schedule
 * is used instead of a computed schedule.
 *
< ASCII >
 * Each of these bands B is then tiled according to "tile" sizes, resulting
 * in two nested bands, with a kernel marker on top
 *
 *		K
 *		|
 *		T
 *		|
 *		P
 *
 * We then split off at most 2 parallel dimensions from the T band and
 * at most 3 parallel dimension from the P band
 *
 *		K
 *		|
 *		T
 *		T1
 *		|
 *		T2
 *		|
 *		P1
 *		|
 *		P2
< ASCII >
 *
 * A filter is introduced in front of T1 that maps the domain instances
 * to block identifiers.  Similarly, a filter is introduced in front of P1
 * that maps the domain instances to thread identifiers.
 *
 * For each iteration of the T2 band and for each array, we compute
 * the array elements accessed by that iteration, construct a rectangular
 * box around it and shift it to the origin.  The result is used
 * as shared memory for the array.
 *
 * Copying and synchronization statements are added to this schedule tree.
 * In principle, these are added in front of the P1 band, but some of
 * them may get hoisted up to higher levels.
 *
 * The entire AST is then generated from the single resulting schedule tree.
 * During the generation the subtrees at kernel nodes (K) are saved
 * aside and replaced by kernel calls.  The result is printed as host code
 * while the saved subtrees are printed as device code.
 */
```
## Visual type:
- #custom


== ./llvm/llvm_215.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/ppcg/hybrid.c#L565-L599

```c
/* Construct the basic hexagonal tile shape.
 * "space" is the 2D space in which the hexagon should be constructed.
 * h is st-1, with st the tile size in the time dimension
 * s0 is the tile size in the space dimension
 * dl is a bound on the negative relative dependence distances, i.e.,
 *
 *	d_s >= -dl d_t
 *
 * du is a bound on the positive relative dependence distances, i.e.,
 *
 *	d_s <= du d_t
 *
 * with (d_t,d_s) any dependence distance vector.
 * dlh = floor(dl * h)
 * duh = floor(du * h)
 *
< ASCII >
 * The shape of the hexagon is as follows:
 *
 *		0 dlh   dlh+s0-1
 *		   ______                __
 * 0		  /      \_             /
 *		 /         \_          /
 * h		/            \ ______ /
 * h+1		\_           //      \\_
 *		  \_        //         \\_
 * 2h+1		    \______//            \\
 *		0   duh   duh+s0-1
 *		             duh+s0-1+dlh
 *		                  duh+s0-1+dlh+1+s0+1
< ASCII >
 *
 * The next hexagon is shifted by duh + dlh + 2 * s0.
 *
 * The slope of the "/" constraints is dl.
 * The slope of the "\_" constraints is du.
 */
```
## Visual type:
- #custom


== ./llvm/llvm_216.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/ppcg/hybrid.c#L2044-L2081

```c
/* Given valid bounds on the relative dependence distances for
 * the pair of nested nodes that "node" point to, as well as sufficiently
 * wide tile sizes "sizes", insert the corresponding time and space tiling
 * at "node", along with a pair of phase nodes that can be used
 * to make further changes.
 * The space of "sizes" should be the product of the spaces
 * of the schedules of the pair of parent and child nodes.
 * "options" determines whether full tiles should be separated
 * from partial tiles.
 *
< ASCII >
 * In particular, given an input of the form
 *
 *	P - C - ...
 *
 * the output has the form
 *
 *	        /- F0 - M0 - CT0 - P - C - ...
 *	PT - seq
 *	        \- F1 - M1 - CT1 - P - C - ...
< ASCII >
 *
 * PT is the global time tiling.  Within each of these tiles,
 * two phases are executed in order.  Within each phase, the schedule
 * space is further subdivided into tiles through CT0 and CT1.
 * The first dimension of each of these iterates over the hexagons
 * within a phase and these are independent by construction.
 * The F0 and F1 filters filter the statement instances that belong
 * to the corresponding phase.  The M0 and M1 marks contain a pointer
 * to a ppcg_ht_phase object that can be used to perform further changes.
 *
 * After checking that input satisfies the requirements,
 * a data structure is constructed that represents the tiling and
 * two additional data structures are constructed for the two phases
 * of the tiling.  These are then used to define the filters F0 and F1 and
 * combined to construct the time tiling PT.
 * Then the time tiling node PT is inserted, followed by
 * the sequence with the two filters, the CT space tiling nodes and
 * the phase markers M.
 */
```
## Visual type:
- #custom


== ./llvm/llvm_217.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/ppcg/hybrid.c#L2153-L2164

```c
/* Given a branch "node" that contains a sequence node with two phases
 * of hybrid tiling as input, call "fn" on each of the two phase marker
 * nodes.
 *
< ASCII >
 * That is, the input is as follows
 *
 *	         /- F0 - M0 - ...
 *	... - seq
 *	         \- F1 - M1 - ...
< ASCII >
 *
 * and "fn" is called on M0 and on M1.
 */
```
## Visual type:
- #custom


== ./llvm/llvm_218.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/polly/lib/External/ppcg/hybrid.c#L2223-L2237

```c
/* Given a branch "node" that contains a sequence node with two phases
 * of hybrid tiling as input, remove the two phase marker nodes.
 *
< ASCII >
 * That is, the input is as follows
 *
 *	         /- F0 - M0 - ...
 *	... - seq
 *	         \- F1 - M1 - ...
 *
 * and the output is
 *
 *	         /- F0 - ...
 *	... - seq
 *	         \- F1 - ...
< ASCII >
 */
```
## Visual type:
- #custom


== ./llvm/llvm_219.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/third-party/unittest/googlemock/src/gmock-matchers.cc#L60-L121

```c
// FindMaxBipartiteMatching and its helper class.
//
// Uses the well-known Ford-Fulkerson max flow method to find a maximum
// bipartite matching. Flow is considered to be from left to right.
// There is an implicit source node that is connected to all of the left
// nodes, and an implicit sink node that is connected to all of the
// right nodes. All edges have unit capacity.
//
// Neither the flow graph nor the residual flow graph are represented
// explicitly. Instead, they are implied by the information in 'graph' and
// a vector<int> called 'left_' whose elements are initialized to the
// value kUnused. This represents the initial state of the algorithm,
// where the flow graph is empty, and the residual flow graph has the
// following edges:
//   - An edge from source to each left_ node
//   - An edge from each right_ node to sink
//   - An edge from each left_ node to each right_ node, if the
//     corresponding edge exists in 'graph'.
//
// When the TryAugment() method adds a flow, it sets left_[l] = r for some
// nodes l and r. This induces the following changes:
//   - The edges (source, l), (l, r), and (r, sink) are added to the
//     flow graph.
//   - The same three edges are removed from the residual flow graph.
//   - The reverse edges (l, source), (r, l), and (sink, r) are added
//     to the residual flow graph, which is a directional graph
//     representing unused flow capacity.
//
// When the method augments a flow (moving left_[l] from some r1 to some
// other r2), this can be thought of as "undoing" the above steps with
// respect to r1 and "redoing" them with respect to r2.
//
// It bears repeating that the flow graph and residual flow graph are
// never represented explicitly, but can be derived by looking at the
// information in 'graph' and in left_.
//
// As an optimization, there is a second vector<int> called right_ which
// does not provide any new information. Instead, it enables more
// efficient queries about edges entering or leaving the right-side nodes
// of the flow or residual flow graphs. The following invariants are
// maintained:
//
// left[l] == kUnused or right[left[l]] == l
// right[r] == kUnused or left[right[r]] == r
//
< ASCII >
// . [ source ]                                        .
// .   |||                                             .
// .   |||                                             .
// .   ||\--> left[0]=1  ---\    right[0]=-1 ----\     .
// .   ||                   |                    |     .
// .   |\---> left[1]=-1    \--> right[1]=0  ---\|     .
// .   |                                        ||     .
// .   \----> left[2]=2  ------> right[2]=2  --\||     .
// .                                           |||     .
// .         elements           matchers       vvv     .
// .                                         [ sink ]  .
< ASCII >
//
// See Also:
//   [1] Cormen, et al (2001). "Section 26.2: The Ford-Fulkerson method".
//       "Introduction to Algorithms (Second ed.)", pp. 651-664.
//   [2] "Ford-Fulkerson algorithm", Wikipedia,
//       'http://en.wikipedia.org/wiki/Ford%E2%80%93Fulkerson_algorithm'
```
## Visual type:
- #custom


== ./llvm/llvm_22.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L128-L130

```c
< ASCII >
// Load/store exclusive
// | size (2) 00 | 1000 | o2 L o1 | Rs (5) | o0 | Rt2 (5) | Rn (5) | Rt (5) |
// L == 0 for Stores.
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_23.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L139-L140

```c
< ASCII >
// Load register literal
// | opc (2) 01 | 1 V 00 | imm19 | Rt (5) |
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_24.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L163-L164

```c
// (offset)
< ASCII >
// | opc (2) 10 | 1 V 01 | 0 L | imm7 | Rt2 (5) | Rn (5) | Rt (5) |
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_25.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L169-L171

```c
// (pre-index)
< ASCII >
// | opc (2) 10 | 1 V 01 | 1 L | imm7 | Rt2 (5) | Rn (5) | Rt (5) |
< ASCII >
// Writes to Rn.
```
## Visual type:
- #memory-layout


== ./llvm/llvm_26.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L180-L182

```c
< ASCII >
// Load/store register (unscaled immediate)
// | size (2) 11 | 1 V 00 | opc (2) 0 | imm9 | 00 | Rn (5) | Rt (5) |
// V == 0 for Scalar, V == 1 for Simd/FP.
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_27.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L187-L188

```c
< ASCII >
// Load/store register (immediate post-indexed)
// | size (2) 11 | 1 V 00 | opc (2) 0 | imm9 | 01 | Rn (5) | Rt (5) |
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_28.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L193-L194

```c
< ASCII >
// Load/store register (unprivileged)
// | size (2) 11 | 1 V 00 | opc (2) 0 | imm9 | 10 | Rn (5) | Rt (5) |
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_29.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L199-L200

```c
< ASCII >
// Load/store register (immediate pre-indexed)
// | size (2) 11 | 1 V 00 | opc (2) 0 | imm9 | 11 | Rn (5) | Rt (5) |
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_3.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/flang/lib/Semantics/resolve-names.cpp#L133-L145

```c
< ASCII >
// Inheritance graph for the parse tree visitation classes that follow:
//   BaseVisitor
//   + AttrsVisitor
//   | + DeclTypeSpecVisitor
//   |   + ImplicitRulesVisitor
//   |     + ScopeHandler -----------+--+
//   |       + ModuleVisitor ========|==+
//   |       + InterfaceVisitor      |  |
//   |       +-+ SubprogramVisitor ==|==+
//   + ArraySpecVisitor              |  |
//     + DeclarationVisitor <--------+  |
//       + ConstructVisitor             |
//         + ResolveNamesVisitor <------+
< ASCII >
```
## Visual type:
- #inheritance-graph


== ./llvm/llvm_30.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L205-L206

```c
// Load/store register (register offset)
< ASCII >
// | size (2) 11 | 1 V 00 | opc (2) 1 | Rm (5) | option (3) S | 10 | Rn | Rt |
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_31.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/AArch64ErrataFix.cpp#L211-L212

```c
// Load/store register (unsigned immediate)
< ASCII >
// | size (2) 11 | 1 V 01 | opc (2) | imm12 | Rn (5) | Rt (5) |
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_32.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/ARMErrataFix.cpp#L99-L107

```c
// Return true if the half-word, when taken as the first of a pair of halfwords
// is the first half of a 32-bit instruction.
// Reference from ARM Architecture Reference Manual ARMv7-A and ARMv7-R edition
// section A6.3: 32-bit Thumb instruction encoding
< ASCII >
// |             HW1                   |               HW2                |
// | 1 1 1 | op1 (2) | op2 (7) | x (4) |op|           x (15)              |
< ASCII >
// With op1 == 0b00, a 16-bit instruction is encoded.
//
// We test only the first halfword, looking for op != 0b00.
```
## Visual type:
- #memory-layout


== ./llvm/llvm_33.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/ELF/ARMErrataFix.cpp#L112-L119

```c
// Reference from ARM Architecture Reference Manual ARMv7-A and ARMv7-R edition
// section A6.3.4 Branches and miscellaneous control.
< ASCII >
// |             HW1              |               HW2                |
// | 1 1 1 | 1 0 | op (7) | x (4) | 1 | op1 (3) | op2 (4) | imm8 (8) |
// op1 == 0x0 op != x111xxx | Conditional branch (Bcc.W)
// op1 == 0x1               | Branch (B.W)
// op1 == 1x0               | Branch with Link and Exchange (BLX.w)
// op1 == 1x1               | Branch with Link (BL.W)
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_34.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/MachO/ExportTrie.cpp#L1-L35

```c
//===- ExportTrie.cpp -----------------------------------------------------===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// This is a partial implementation of the Mach-O export trie format. It's
// essentially a symbol table encoded as a compressed prefix trie, meaning that
// the common prefixes of each symbol name are shared for a more compact
// representation. The prefixes are stored on the edges of the trie, and one
// edge can represent multiple characters. For example, given two exported
// symbols _bar and _baz, we will have a trie like this (terminal nodes are
// marked with an asterisk):
//
< ASCII >
//              +-+-+
//              |   | // root node
//              +-+-+
//                |
//                | _ba
//                |
//              +-+-+
//              |   |
//              +-+-+
//           r /     \ z
//            /       \
//        +-+-+       +-+-+
//        | * |       | * |
//        +-+-+       +-+-+
< ASCII >
//
// More documentation of the format can be found in
// llvm/tools/obj2yaml/macho2yaml.cpp.
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #tree


== ./llvm/llvm_35.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/MachO/Arch/ARM64Common.h#L40-L43

```c
< ASCII >
//              25                                                0
// +-----------+---------------------------------------------------+
// |           |                       imm26                       |
// +-----------+---------------------------------------------------+
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_36.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/MachO/Arch/ARM64Common.h#L59-L62

```c
< ASCII >
//   30 29          23                                  5
// +-+---+---------+-------------------------------------+---------+
// | |ilo|         |                immhi                |         |
// +-+---+---------+-------------------------------------+---------+
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_37.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/MachO/Arch/ARM64Common.h#L81-L84

```c
< ASCII >
//                      21                   10
// +-------------------+-----------------------+-------------------+
// |                   |         imm12         |                   |
// +-------------------+-----------------------+-------------------+
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_38.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/lld/test/ELF/ctors_dtors_priority.s#L57-L61

```c
< ASCII >
// CHECK:      Contents of section .ctors:
// CHECK-NEXT:        a1000000 00000000 01000000 00000000
// CHECK-NEXT:        04000000 00000000 05000000 00000000
// CHECK-NEXT:        b1000000 00000000 03000000 00000000
// CHECK-NEXT:        02000000 00000000 c1000000 00000000
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_39.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/ADT/GenericUniformityImpl.h#L151-L258

```c
// --- Above line is not a doxygen comment; intentionally left blank ---
//
// Originally implemented in SyncDependenceAnalysis.cpp for DivergenceAnalysis.
//
// The SyncDependenceAnalysis is used in the UniformityAnalysis to model
// control-induced divergence in phi nodes.
//
// -- Reference --
// The algorithm is an extension of Section 5 of
//
//   An abstract interpretation for SPMD divergence
//       on reducible control flow graphs.
//   Julian Rosemann, Simon Moll and Sebastian Hack
//   POPL '21
//
//
// -- Sync dependence --
// Sync dependence characterizes the control flow aspect of the
// propagation of branch divergence. For example,
//
//   %cond = icmp slt i32 %tid, 10
//   br i1 %cond, label %then, label %else
// then:
//   br label %merge
// else:
//   br label %merge
// merge:
//   %a = phi i32 [ 0, %then ], [ 1, %else ]
//
// Suppose %tid holds the thread ID. Although %a is not data dependent on %tid
// because %tid is not on its use-def chains, %a is sync dependent on %tid
// because the branch "br i1 %cond" depends on %tid and affects which value %a
// is assigned to.
//
//
// -- Reduction to SSA construction --
// There are two disjoint paths from A to X, if a certain variant of SSA
// construction places a phi node in X under the following set-up scheme.
//
// This variant of SSA construction ignores incoming undef values.
// That is paths from the entry without a definition do not result in
// phi nodes.
//
< ASCII >
//       entry
//     /      \
//    A        \
//  /   \       Y
// B     C     /
//  \   /  \  /
//    D     E
//     \   /
//       F
< ASCII >
//
// Assume that A contains a divergent branch. We are interested
// in the set of all blocks where each block is reachable from A
// via two disjoint paths. This would be the set {D, F} in this
// case.
// To generally reduce this query to SSA construction we introduce
// a virtual variable x and assign to x different values in each
// successor block of A.
//
< ASCII >
//           entry
//         /      \
//        A        \
//      /   \       Y
// x = 0   x = 1   /
//      \  /   \  /
//        D     E
//         \   /
//           F
//
// Our flavor of SSA construction for x will construct the following
//
//            entry
//          /      \
//         A        \
//       /   \       Y
// x0 = 0   x1 = 1  /
//       \   /   \ /
//     x2 = phi   E
//         \     /
//         x3 = phi
< ASCII >
//
// The blocks D and F contain phi nodes and are thus each reachable
// by two disjoins paths from A.
//
// -- Remarks --
// * In case of cycle exits we need to check for temporal divergence.
//   To this end, we check whether the definition of x differs between the
//   cycle exit and the cycle header (_after_ SSA construction).
//
// * In the presence of irreducible control flow, the fixed point is
//   reached only after multiple iterations. This is because labels
//   reaching the header of a cycle must be repropagated through the
//   cycle. This is true even in a reducible cycle, since the labels
//   may have been produced by a nested irreducible cycle.
//
// * Note that SyncDependenceAnalysis is not concerned with the points
//   of convergence in an irreducible cycle. It's only purpose is to
//   identify join blocks. The "diverged entry" criterion is
//   separately applied on join blocks to determine if an entire
//   irreducible cycle is assumed to be divergent.
//
// * Relevant related work:
//     A simple algorithm for global data flow analysis problems.
//     Matthew S. Hecht and Jeffrey D. Ullman.
//     SIAM Journal on Computing, 4(4):519–532, December 1975.
//
```
## Visual type:
- #program-graph


== ./llvm/llvm_4.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libc/src/__support/FPUtil/x86_64/FEnvImpl.h#L394-L462

```c
/*
    fenv_t control word format:

    Windows (at least for x64) uses a 4 byte control fenv control word stored in
    a 32 bit integer. The first byte contains just the rounding mode and the
    exception masks, while the last two bytes contain that same information as
    well as the flush-to-zero and denormals-are-zero flags. The flags are
    represented with a truth table:

    00 - No flags set
    01 - Flush-to-zero and Denormals-are-zero set
    11 - Flush-to-zero set
    10 - Denormals-are-zero set

< ASCII >
    U represents unused.

     +-----Rounding Mode-----+
     |                       |
    ++                      ++
    ||                      ||
    RRMMMMMM UUUUUUUU UUUUFFRR UUMMMMMM
      |    |              ||     |    |
      +----+      flags---++     +----+
           |                          |
           +------Exception Masks-----+


    fenv_t status word format:

    The status word is a lot simpler for this conversion, since only the
    exception flags are used in the MXCSR.

      +----+---Exception Flags---+----+
      |    |                     |    |
    UUEEEEEE UUUUUUUU UUUUUUUU UUEEEEEE



    MXCSR Format:

    The MXCSR format is the same information, just organized differently. Since
    the fenv_t struct for windows doesn't include the mxcsr bits, they must be
    generated from the control word bits.

      Exception Masks---+           +---Exception Flags
                        |           |
     Flush-to-zero---+  +----+ +----+
                     |  |    | |    |
                     FRRMMMMMMDEEEEEE
                      ||      |
                      ++      +---Denormals-are-zero
                      |
                      +---Rounding Mode
< ASCII >


    The mask and flag order is as follows:

    fenv_t      mxcsr

    denormal    inexact
    invalid     underflow
    div by 0    overflow
    overflow    div by 0
    underflow   denormal
    inexact     invalid

    This is almost reverse, except for denormal and invalid which are in the
    same order in both.
  */
```
## Visual type:
- #memory-layout


== ./llvm/llvm_40.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/ADT/IntervalTree.h#L28-L188

```c
// IntervalTree is a light tree data structure to hold intervals. It allows
// finding all intervals that overlap with any given point. At this time,
// it does not support any deletion or rebalancing operations.
//
// The IntervalTree is designed to be set up once, and then queried without
// any further additions.
//
// Synopsis:
//   Closed intervals delimited by PointT objects are mapped to ValueT objects.
//
// Restrictions:
//   PointT must be a fundamental type.
//   ValueT must be a fundamental or pointer type.
//
// template <typename PointT, typename ValueT, typename DataT>
// class IntervalTree {
// public:
//
//   IntervalTree();
//   ~IntervalTree():
//
//   using IntervalReferences = SmallVector<IntervalData *>;
//
//   void create();
//   void insert(PointT Left, PointT Right, ValueT Value);
//
//   IntervalReferences getContaining(PointT Point);
//   static void sortIntervals(IntervalReferences &Intervals, Sorting Sort);
//
//   find_iterator begin(PointType Point) const;
//   find_iterator end() const;
//
//   bool empty() const;
//   void clear();
//
//   void print(raw_ostream &OS, bool HexFormat = true);
// };
//
//===----------------------------------------------------------------------===//
//
// In the below given dataset
//
//   [a, b] <- (x)
//
// 'a' and 'b' describe a range and 'x' the value for that interval.
//
< ASCII >
// The following data are purely for illustrative purposes:
//
// [30, 35] <- (3035),    [39, 50] <- (3950),    [55, 61] <- (5561),
// [31, 56] <- (3156),    [12, 21] <- (1221),    [25, 41] <- (2541),
// [49, 65] <- (4965),    [71, 79] <- (7179),    [11, 16] <- (1116),
// [20, 30] <- (2030),    [36, 54] <- (3654),    [60, 70] <- (6070),
// [74, 80] <- (7480),    [15, 40] <- (1540),    [43, 43] <- (4343),
// [50, 75] <- (5075),    [10, 85] <- (1085)
//
// The data represents a set of overlapping intervals:
//
//                    30--35  39------------50  55----61
//                      31------------------------56
//     12--------21 25------------41      49-------------65   71-----79
//   11----16  20-----30    36----------------54    60------70  74---- 80
//       15---------------------40  43--43  50--------------------75
// 10----------------------------------------------------------------------85
< ASCII >
//
// The items are stored in a binary tree with each node storing:
//
// MP: A middle point.
// IL: All intervals whose left value are completely to the left of the middle
//     point. They are sorted in ascending order by their beginning point.
// IR: All intervals whose right value are completely to the right of the
//     middle point. They are sorted in descending order by their ending point.
// LS: Left subtree.
// RS: Right subtree.
//
// As IL and IR will contain the same intervals, in order to optimize space,
// instead of storing intervals on each node, we use two vectors that will
// contain the intervals described by IL and IR. Each node will contain an
// index into that vector (global bucket), to indicate the beginning of the
// intervals assigned to the node.
//
// The following is the output from print():
//
// 0: MP:43 IR [10,85] [31,56] [36,54] [39,50] [43,43]
// 0: MP:43 IL [10,85] [31,56] [36,54] [39,50] [43,43]
// 1:   MP:25 IR [25,41] [15,40] [20,30]
// 1:   MP:25 IL [15,40] [20,30] [25,41]
// 2:     MP:15 IR [12,21] [11,16]
// 2:     MP:15 IL [11,16] [12,21]
// 2:     MP:36 IR []
// 2:     MP:36 IL []
// 3:       MP:31 IR [30,35]
// 3:       MP:31 IL [30,35]
// 1:   MP:61 IR [50,75] [60,70] [49,65] [55,61]
// 1:   MP:61 IL [49,65] [50,75] [55,61] [60,70]
// 2:     MP:74 IR [74,80] [71,79]
// 2:     MP:74 IL [71,79] [74,80]
//
// with:
//    0: Root Node.
//   MP: Middle point.
//   IL: Intervals to the left (in ascending order by beginning point).
//   IR: Intervals to the right (in descending order by ending point).
//
< ASCII >
//                                    Root
//                                      |
//                                      V
//                       +------------MP:43------------+
//                       |            IL IR            |
//                       |       [10,85] [10,85]       |
//                    LS |       [31,56] [31,56]       | RS
//                       |       [36,54] [36,54]       |
//                       |       [39,50] [39,50]       |
//                       |       [43,43] [43,43]       |
//                       V                             V
//        +------------MP:25------------+            MP:61------------+
//        |            IL IR            |            IL IR            |
//        |       [15,40] [25,41]       |       [49,65] [50,75]       |
//     LS |       [20,30] [15,40]       | RS    [50,75] [60,70]       | RS
//        |       [25,41] [20,30]       |       [55,61] [49,65]       |
//        |                             |       [60,70] [55,61]       |
//        V                             V                             V
//      MP:15                 +-------MP:36                         MP:74
//      IL IR                 |       IL IR                         IL IR
// [11,16] [12,21]         LS |       [] []                    [71,79] [74,80]
// [12,21] [11,16]            |                                [74,80] [71,79]
//                            V
//                          MP:31
//                          IL IR
//                     [30,35] [30,35]
< ASCII >
//
// The creation of an interval tree is done in 2 steps:
// 1) Insert the interval items by calling
//    void insert(PointT Left, PointT Right, ValueT Value);
//    Left, Right: the interval left and right limits.
//    Value: the data associated with that specific interval.
//
// 2) Create the interval tree by calling
//    void create();
//
// Once the tree is created, it is switched to query mode.
// Query the tree by using iterators or container.
//
// a) Iterators over intervals overlapping the given point with very weak
//    ordering guarantees.
//    find_iterator begin(PointType Point) const;
//    find_iterator end() const;
//    Point: a target point to be tested for inclusion in any interval.
//
// b) Container:
//    IntervalReferences getContaining(PointT Point);
//    Point: a target point to be tested for inclusion in any interval.
//    Returns vector with all the intervals containing the target point.
//
// The returned intervals are in their natural tree location. They can
// be sorted:
//
// static void sortIntervals(IntervalReferences &Intervals, Sorting Sort);
//
// Ability to print the constructed interval tree:
//   void print(raw_ostream &OS, bool HexFormat = true);
// Display the associated data in hexadecimal format.
```
## Visual type:
- #linear-intervals


== ./llvm/llvm_41.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/Analysis/MustExecute.h#L182-L271

```c
/// Must be executed iterators visit stretches of instructions that are
/// guaranteed to be executed together, potentially with other instruction
/// executed in-between.
///
/// Given the following code, and assuming all statements are single
/// instructions which transfer execution to the successor (see
/// isGuaranteedToTransferExecutionToSuccessor), there are two possible
/// outcomes. If we start the iterator at A, B, or E, we will visit only A, B,
/// and E. If we start at C or D, we will visit all instructions A-E.
///
/// \code
///   A;
///   B;
///   if (...) {
///     C;
///     D;
///   }
///   E;
/// \endcode
///
///
/// Below is the example extneded with instructions F and G. Now we assume F
/// might not transfer execution to it's successor G. As a result we get the
/// following visit sets:
///
< ASCII >
/// Start Instruction   | Visit Set
/// A                   | A, B,       E, F
///    B                | A, B,       E, F
///       C             | A, B, C, D, E, F
///          D          | A, B, C, D, E, F
///             E       | A, B,       E, F
///                F    | A, B,       E, F
///                   G | A, B,       E, F, G
< ASCII >
///
///
/// \code
///   A;
///   B;
///   if (...) {
///     C;
///     D;
///   }
///   E;
///   F;  // Might not transfer execution to its successor G.
///   G;
/// \endcode
///
///
/// A more complex example involving conditionals, loops, break, and continue
/// is shown below. We again assume all instructions will transmit control to
/// the successor and we assume we can prove the inner loop to be finite. We
/// omit non-trivial branch conditions as the exploration is oblivious to them.
/// Constant branches are assumed to be unconditional in the CFG. The resulting
/// visist sets are shown in the table below.
///
/// \code
///   A;
///   while (true) {
///     B;
///     if (...)
///       C;
///     if (...)
///       continue;
///     D;
///     if (...)
///       break;
///     do {
///       if (...)
///         continue;
///       E;
///     } while (...);
///     F;
///   }
///   G;
/// \endcode
///
< ASCII >
/// Start Instruction    | Visit Set
/// A                    | A, B
///    B                 | A, B
///       C              | A, B, C
///          D           | A, B,    D
///             E        | A, B,    D, E, F
///                F     | A, B,    D,    F
///                   G  | A, B,    D,       G
< ASCII >
///
///
/// Note that the examples show optimal visist sets but not necessarily the ones
/// derived by the explorer depending on the available CFG analyses (see
/// MustBeExecutedContextExplorer). Also note that we, depending on the options,
/// the visit set can contain instructions from other functions.
```
## Visual type:
- #table


== ./llvm/llvm_42.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/Analysis/ValueTracking.h#L818-L827

```c
/// Return true if RHS is known to be implied true by LHS.  Return false if
/// RHS is known to be implied false by LHS.  Otherwise, return std::nullopt if
/// no implication can be made. A & B must be i1 (boolean) values or a vector of
/// such values. Note that the truth table for implication is the same as <=u on
/// i1 values (but not
/// <=s!).  The truth table for both is:
< ASCII >
///    | T | F (B)
///  T | T | F
///  F | T | T
< ASCII >
/// (A)
```
## Visual type:
- #table


== ./llvm/llvm_43.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/BinaryFormat/DXContainer.h#L27-L35

```c
< ASCII >
// ┌────────────────────────────────┐
// │             Header             │
// ├────────────────────────────────┤
// │              Part              │
// ├────────────────────────────────┤
// │              Part              │
// ├────────────────────────────────┤
// │              ...               │
// └────────────────────────────────┘
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_44.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/CodeGen/RDFGraph.h#L1-L222

```c
//===- RDFGraph.h -----------------------------------------------*- C++ -*-===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// Target-independent, SSA-based data flow graph for register data flow (RDF)
// for a non-SSA program representation (e.g. post-RA machine code).
//
//
// *** Introduction
//
// The RDF graph is a collection of nodes, each of which denotes some element
// of the program. There are two main types of such elements: code and refe-
// rences. Conceptually, "code" is something that represents the structure
// of the program, e.g. basic block or a statement, while "reference" is an
// instance of accessing a register, e.g. a definition or a use. Nodes are
// connected with each other based on the structure of the program (such as
// blocks, instructions, etc.), and based on the data flow (e.g. reaching
// definitions, reached uses, etc.). The single-reaching-definition principle
// of SSA is generally observed, although, due to the non-SSA representation
// of the program, there are some differences between the graph and a "pure"
// SSA representation.
//
//
// *** Implementation remarks
//
// Since the graph can contain a large number of nodes, memory consumption
// was one of the major design considerations. As a result, there is a single
// base class NodeBase which defines all members used by all possible derived
// classes. The members are arranged in a union, and a derived class cannot
// add any data members of its own. Each derived class only defines the
// functional interface, i.e. member functions. NodeBase must be a POD,
// which implies that all of its members must also be PODs.
// Since nodes need to be connected with other nodes, pointers have been
// replaced with 32-bit identifiers: each node has an id of type NodeId.
// There are mapping functions in the graph that translate between actual
// memory addresses and the corresponding identifiers.
// A node id of 0 is equivalent to nullptr.
//
//
// *** Structure of the graph
//
// A code node is always a collection of other nodes. For example, a code
// node corresponding to a basic block will contain code nodes corresponding
// to instructions. In turn, a code node corresponding to an instruction will
// contain a list of reference nodes that correspond to the definitions and
// uses of registers in that instruction. The members are arranged into a
// circular list, which is yet another consequence of the effort to save
// memory: for each member node it should be possible to obtain its owner,
// and it should be possible to access all other members. There are other
// ways to accomplish that, but the circular list seemed the most natural.
//
< ASCII >
// +- CodeNode -+
// |            | <---------------------------------------------------+
// +-+--------+-+                                                     |
//   |FirstM  |LastM                                                  |
//   |        +-------------------------------------+                 |
//   |                                              |                 |
//   V                                              V                 |
//  +----------+ Next +----------+ Next       Next +----------+ Next  |
//  |          |----->|          |-----> ... ----->|          |----->-+
//  +- Member -+      +- Member -+                 +- Member -+
< ASCII >
//
// The order of members is such that related reference nodes (see below)
// should be contiguous on the member list.
//
// A reference node is a node that encapsulates an access to a register,
// in other words, data flowing into or out of a register. There are two
// major kinds of reference nodes: defs and uses. A def node will contain
// the id of the first reached use, and the id of the first reached def.
// Each def and use will contain the id of the reaching def, and also the
// id of the next reached def (for def nodes) or use (for use nodes).
// The "next node sharing the same reaching def" is denoted as "sibling".
// In summary:
// - Def node contains: reaching def, sibling, first reached def, and first
// reached use.
// - Use node contains: reaching def and sibling.
//
< ASCII >
// +-- DefNode --+
// | R2 = ...    | <---+--------------------+
// ++---------+--+     |                    |
//  |Reached  |Reached |                    |
//  |Def      |Use     |                    |
//  |         |        |Reaching            |Reaching
//  |         V        |Def                 |Def
//  |      +-- UseNode --+ Sib  +-- UseNode --+ Sib       Sib
//  |      | ... = R2    |----->| ... = R2    |----> ... ----> 0
//  |      +-------------+      +-------------+
//  V
// +-- DefNode --+ Sib
// | R2 = ...    |----> ...
// ++---------+--+
//  |         |
//  |         |
// ...       ...
< ASCII >
//
// To get a full picture, the circular lists connecting blocks within a
// function, instructions within a block, etc. should be superimposed with
// the def-def, def-use links shown above.
// To illustrate this, consider a small example in a pseudo-assembly:
// foo:
//   add r2, r0, r1   ; r2 = r0+r1
//   addi r0, r2, 1   ; r0 = r2+1
//   ret r0           ; return value in r0
//
// The graph (in a format used by the debugging functions) would look like:
//
//   DFG dump:[
//   f1: Function foo
//   b2: === %bb.0 === preds(0), succs(0):
//   p3: phi [d4<r0>(,d12,u9):]
//   p5: phi [d6<r1>(,,u10):]
//   s7: add [d8<r2>(,,u13):, u9<r0>(d4):, u10<r1>(d6):]
//   s11: addi [d12<r0>(d4,,u15):, u13<r2>(d8):]
//   s14: ret [u15<r0>(d12):]
//   ]
//
// The f1, b2, p3, etc. are node ids. The letter is prepended to indicate the
// kind of the node (i.e. f - function, b - basic block, p - phi, s - state-
// ment, d - def, u - use).
// The format of a def node is:
//   dN<R>(rd,d,u):sib,
// where
//   N   - numeric node id,
//   R   - register being defined
//   rd  - reaching def,
//   d   - reached def,
//   u   - reached use,
//   sib - sibling.
// The format of a use node is:
//   uN<R>[!](rd):sib,
// where
//   N   - numeric node id,
//   R   - register being used,
//   rd  - reaching def,
//   sib - sibling.
// Possible annotations (usually preceding the node id):
//   +   - preserving def,
//   ~   - clobbering def,
//   "   - shadow ref (follows the node id),
//   !   - fixed register (appears after register name).
//
// The circular lists are not explicit in the dump.
//
//
// *** Node attributes
//
// NodeBase has a member "Attrs", which is the primary way of determining
// the node's characteristics. The fields in this member decide whether
// the node is a code node or a reference node (i.e. node's "type"), then
// within each type, the "kind" determines what specifically this node
// represents. The remaining bits, "flags", contain additional information
// that is even more detailed than the "kind".
// CodeNode's kinds are:
// - Phi:   Phi node, members are reference nodes.
// - Stmt:  Statement, members are reference nodes.
// - Block: Basic block, members are instruction nodes (i.e. Phi or Stmt).
// - Func:  The whole function. The members are basic block nodes.
// RefNode's kinds are:
// - Use.
// - Def.
//
// Meaning of flags:
// - Preserving: applies only to defs. A preserving def is one that can
//   preserve some of the original bits among those that are included in
//   the register associated with that def. For example, if R0 is a 32-bit
//   register, but a def can only change the lower 16 bits, then it will
//   be marked as preserving.
// - Shadow: a reference that has duplicates holding additional reaching
//   defs (see more below).
// - Clobbering: applied only to defs, indicates that the value generated
//   by this def is unspecified. A typical example would be volatile registers
//   after function calls.
// - Fixed: the register in this def/use cannot be replaced with any other
//   register. A typical case would be a parameter register to a call, or
//   the register with the return value from a function.
// - Undef: the register in this reference the register is assumed to have
//   no pre-existing value, even if it appears to be reached by some def.
//   This is typically used to prevent keeping registers artificially live
//   in cases when they are defined via predicated instructions. For example:
//     r0 = add-if-true cond, r10, r11                (1)
//     r0 = add-if-false cond, r12, r13, implicit r0  (2)
//     ... = r0                                       (3)
//   Before (1), r0 is not intended to be live, and the use of r0 in (3) is
//   not meant to be reached by any def preceding (1). However, since the
//   defs in (1) and (2) are both preserving, these properties alone would
//   imply that the use in (3) may indeed be reached by some prior def.
//   Adding Undef flag to the def in (1) prevents that. The Undef flag
//   may be applied to both defs and uses.
// - Dead: applies only to defs. The value coming out of a "dead" def is
//   assumed to be unused, even if the def appears to be reaching other defs
//   or uses. The motivation for this flag comes from dead defs on function
//   calls: there is no way to determine if such a def is dead without
//   analyzing the target's ABI. Hence the graph should contain this info,
//   as it is unavailable otherwise. On the other hand, a def without any
//   uses on a typical instruction is not the intended target for this flag.
//
// *** Shadow references
//
// It may happen that a super-register can have two (or more) non-overlapping
// sub-registers. When both of these sub-registers are defined and followed
// by a use of the super-register, the use of the super-register will not
// have a unique reaching def: both defs of the sub-registers need to be
// accounted for. In such cases, a duplicate use of the super-register is
// added and it points to the extra reaching def. Both uses are marked with
// a flag "shadow". Example:
// Assume t0 is a super-register of r0 and r1, r0 and r1 do not overlap:
//   set r0, 1        ; r0 = 1
//   set r1, 1        ; r1 = 1
//   addi t1, t0, 1   ; t1 = t0+1
//
// The DFG:
//   s1: set [d2<r0>(,,u9):]
//   s3: set [d4<r1>(,,u10):]
//   s5: addi [d6<t1>(,,):, u7"<t0>(d2):, u8"<t0>(d4):]
//
// The statement s5 has two use nodes for t0: u7" and u9". The quotation
// mark " indicates that the node is a shadow.
//
```
## Visual type:
- #program-graph


== ./llvm/llvm_45.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/Support/ARMWinEH.h#L32-L94

```c
/// RuntimeFunction - An entry in the table of procedure data (.pdata)
///
/// This is ARM specific, but the Function Start RVA, Flag and
/// ExceptionInformationRVA fields work identically for ARM64.
///
< ASCII >
///  3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 0 0 0 0 0 0 0 0 0 0
///  1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0
/// +---------------------------------------------------------------+
/// |                     Function Start RVA                        |
/// +-------------------+-+-+-+-----+-+---+---------------------+---+
/// |    Stack Adjust   |C|L|R| Reg |H|Ret|   Function Length   |Flg|
/// +-------------------+-+-+-+-----+-+---+---------------------+---+
< ASCII >
///
/// Flag : 2-bit field with the following meanings:
///   - 00 = packed unwind data not used; reamining bits point to .xdata record
///   - 01 = packed unwind data
///   - 10 = packed unwind data, function assumed to have no prologue; useful
///          for function fragments that are discontiguous with the start of the
///          function
///   - 11 = reserved
/// Function Length : 11-bit field providing the length of the entire function
///                   in bytes, divided by 2; if the function is greater than
///                   4KB, a full .xdata record must be used instead
/// Ret : 2-bit field indicating how the function returns
///   - 00 = return via pop {pc} (the L bit must be set)
///   - 01 = return via 16-bit branch
///   - 10 = return via 32-bit branch
///   - 11 = no epilogue; useful for function fragments that may only contain a
///          prologue but the epilogue is elsewhere
/// H : 1-bit flag indicating whether the function "homes" the integer parameter
///     registers (r0-r3), allocating 16-bytes on the stack
/// Reg : 3-bit field indicating the index of the last saved non-volatile
///       register.  If the R bit is set to 0, then only integer registers are
///       saved (r4-rN, where N is 4 + Reg).  If the R bit is set to 1, then
///       only floating-point registers are being saved (d8-dN, where N is
///       8 + Reg).  The special case of the R bit being set to 1 and Reg equal
///       to 7 indicates that no registers are saved.
/// R : 1-bit flag indicating whether the non-volatile registers are integer or
///     floating-point.  0 indicates integer, 1 indicates floating-point.  The
///     special case of the R-flag being set and Reg being set to 7 indicates
///     that no non-volatile registers are saved.
/// L : 1-bit flag indicating whether the function saves/restores the link
///     register (LR)
/// C : 1-bit flag indicating whether the function includes extra instructions
///     to setup a frame chain for fast walking.  If this flag is set, r11 is
///     implicitly added to the list of saved non-volatile integer registers.
/// Stack Adjust : 10-bit field indicating the number of bytes of stack that are
///                allocated for this function.  Only values between 0x000 and
///                0x3f3 can be directly encoded.  If the value is 0x3f4 or
///                greater, then the low 4 bits have special meaning as follows:
///                - Bit 0-1
///                  indicate the number of words' of adjustment (1-4), minus 1
///                - Bit 2
///                  indicates if the prologue combined adjustment into push
///                - Bit 3
///                  indicates if the epilogue combined adjustment into pop
///
/// RESTRICTIONS:
///   - IF C is SET:
///     + L flag must be set since frame chaining requires r11 and lr
///     + r11 must NOT be included in the set of registers described by Reg
///   - IF Ret is 0:
///     + L flag must be set
```
## Visual type:
- #memory-layout


== ./llvm/llvm_46.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/Support/ARMWinEH.h#L290-L394

```c
/// ExceptionDataRecord - An entry in the table of exception data (.xdata)
///
< ASCII >
/// The format on ARM is:
///
///  3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 0 0 0 0 0 0 0 0 0 0
///  1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0
/// +-------+---------+-+-+-+---+-----------------------------------+
/// | C Wrd | Epi Cnt |F|E|X|Ver|         Function Length           |
/// +-------+--------+'-'-'-'---'---+-------------------------------+
/// |    Reserved    |Ex. Code Words|   (Extended Epilogue Count)   |
/// +-------+--------+--------------+-------------------------------+
///
/// The format on ARM64 is:
///
///  3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 0 0 0 0 0 0 0 0 0 0
///  1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0
/// +---------+---------+-+-+---+-----------------------------------+
/// |  C Wrd  | Epi Cnt |E|X|Ver|         Function Length           |
/// +---------+------+--'-'-'---'---+-------------------------------+
/// |    Reserved    |Ex. Code Words|   (Extended Epilogue Count)   |
/// +-------+--------+--------------+-------------------------------+
< ASCII >
///
/// Function Length : 18-bit field indicating the total length of the function
///                   in bytes divided by 2.  If a function is larger than
///                   512KB, then multiple pdata and xdata records must be used.
/// Vers : 2-bit field describing the version of the remaining structure.  Only
///        version 0 is currently defined (values 1-3 are not permitted).
/// X : 1-bit field indicating the presence of exception data
/// E : 1-bit field indicating that the single epilogue is packed into the
///     header
/// F : 1-bit field indicating that the record describes a function fragment
///     (implies that no prologue is present, and prologue processing should be
///     skipped) (ARM only)
/// Epilogue Count : 5-bit field that differs in meaning based on the E field.
///
///                  If E is set, then this field specifies the index of the
///                  first unwind code describing the (only) epilogue.
///
///                  Otherwise, this field indicates the number of exception
///                  scopes.  If more than 31 scopes exist, then this field and
///                  the Code Words field must both be set to 0 to indicate that
///                  an extension word is required.
/// Code Words : 4-bit (5-bit on ARM64) field that specifies the number of
///              32-bit words needed to contain all the unwind codes.  If more
///              than 15 words (31 words on ARM64) are required, then this field
///              and the Epilogue Count field must both be set to 0 to indicate
///              that an extension word is required.
/// Extended Epilogue Count, Extended Code Words :
///                          Valid only if Epilog Count and Code Words are both
///                          set to 0.  Provides an 8-bit extended code word
///                          count and 16-bits for epilogue count
///
< ASCII >
/// The epilogue scope format on ARM is:
///
///  3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 0 0 0 0 0 0 0 0 0 0
///  1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0
/// +----------------+------+---+---+-------------------------------+
/// |  Ep Start Idx  | Cond |Res|       Epilogue Start Offset       |
/// +----------------+------+---+-----------------------------------+
///
/// The epilogue scope format on ARM64 is:
///
///  3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 0 0 0 0 0 0 0 0 0 0
///  1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0
/// +-------------------+-------+---+-------------------------------+
/// |  Ep Start Idx     |  Res  |   Epilogue Start Offset           |
/// +-------------------+-------+-----------------------------------+
< ASCII >
///
/// If the E bit is unset in the header, the header is followed by a series of
/// epilogue scopes, which are sorted by their offset.
///
/// Epilogue Start Offset: 18-bit field encoding the offset of epilogue relative
///                        to the start of the function in bytes divided by two
/// Res : 2-bit field reserved for future expansion (must be set to 0)
/// Condition : (ARM only) 4-bit field providing the condition under which the
///             epilogue is executed.  Unconditional epilogues should set this
///             field to 0xe. Epilogues must be entirely conditional or
///             unconditional, and in Thumb-2 mode.  The epilogue begins with
///             the first instruction after the IT opcode.
/// Epilogue Start Index : 8-bit field indicating the byte index of the first
///                        unwind code describing the epilogue
///
< ASCII >
///  3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 0 0 0 0 0 0 0 0 0 0
///  1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0
/// +---------------+---------------+---------------+---------------+
/// | Unwind Code 3 | Unwind Code 2 | Unwind Code 1 | Unwind Code 0 |
/// +---------------+---------------+---------------+---------------+
///
/// Following the epilogue scopes, the byte code describing the unwinding
/// follows.  This is padded to align up to word alignment.  Bytes are stored in
/// little endian.
///
///  3 3 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1 1 1 0 0 0 0 0 0 0 0 0 0
///  1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0
/// +---------------------------------------------------------------+
/// |           Exception Handler RVA (requires X = 1)              |
/// +---------------------------------------------------------------+
/// |  (possibly followed by data required for exception handler)   |
/// +---------------------------------------------------------------+
< ASCII >
///
/// If the X bit is set in the header, the unwind byte code is followed by the
/// exception handler information.  This constants of one Exception Handler RVA
/// which is the address to the exception handler, followed immediately by the
/// variable length data associated with the exception handler.
///
```
## Visual type:
- #memory-layout


== ./llvm/llvm_47.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/Support/Chrono.h#L88-L110

```c
/// Implementation of format_provider<T> for duration types.
///
/// The options string of a duration type has the grammar:
///
///   duration_options  ::= [unit][show_unit [number_options]]
///   unit              ::= `h`|`m`|`s`|`ms|`us`|`ns`
///   show_unit         ::= `+` | `-`
///   number_options    ::= options string for a integral or floating point type
///
< ASCII >
///   Examples
///   =================================
///   |  options  | Input | Output    |
///   =================================
///   | ""        | 1s    | 1 s       |
///   | "ms"      | 1s    | 1000 ms   |
///   | "ms-"     | 1s    | 1000      |
///   | "ms-n"    | 1s    | 1,000     |
///   | ""        | 1.0s  | 1.00 s    |
///   =================================
< ASCII >
///
///  If the unit of the duration type is not one of the units specified above,
///  it is still possible to format it, provided you explicitly request a
///  display unit or you request that the unit is not displayed.
```
## Visual type:
- #table


== ./llvm/llvm_48.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/Support/FormatProviders.h#L102-L123

```c
/// Implementation of format_provider<T> for integral arithmetic types.
///
< ASCII >
/// The options string of an integral type has the grammar:
///
///   integer_options   :: [style][digits]
///   style             :: <see table below>
///   digits            :: <non-negative integer> 0-99
///
///   ==========================================================================
///   |  style  |     Meaning          |      Example     | Digits Meaning     |
///   --------------------------------------------------------------------------
///   |         |                      |  Input |  Output |                    |
///   ==========================================================================
///   |   x-    | Hex no prefix, lower |   42   |    2a   | Minimum # digits   |
///   |   X-    | Hex no prefix, upper |   42   |    2A   | Minimum # digits   |
///   | x+ / x  | Hex + prefix, lower  |   42   |   0x2a  | Minimum # digits   |
///   | X+ / X  | Hex + prefix, upper  |   42   |   0x2A  | Minimum # digits   |
///   | N / n   | Digit grouped number | 123456 | 123,456 | Ignored            |
///   | D / d   | Integer              | 100000 | 100000  | Ignored            |
///   | (empty) | Same as D / d        |        |         |                    |
///   ==========================================================================
< ASCII >
///
```
## Visual type:
- #table


== ./llvm/llvm_49.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/Support/FormatProviders.h#L152-L173

```c
/// Implementation of format_provider<T> for integral pointer types.
///
/// The options string of a pointer type has the grammar:
///
///   pointer_options   :: [style][precision]
///   style             :: <see table below>
///   digits            :: <non-negative integer> 0-sizeof(void*)
///
< ASCII >
///   ==========================================================================
///   |   S     |     Meaning          |                Example                |
///   --------------------------------------------------------------------------
///   |         |                      |       Input       |      Output       |
///   ==========================================================================
///   |   x-    | Hex no prefix, lower |    0xDEADBEEF     |     deadbeef      |
///   |   X-    | Hex no prefix, upper |    0xDEADBEEF     |     DEADBEEF      |
///   | x+ / x  | Hex + prefix, lower  |    0xDEADBEEF     |    0xdeadbeef     |
///   | X+ / X  | Hex + prefix, upper  |    0xDEADBEEF     |    0xDEADBEEF     |
///   | (empty) | Same as X+ / X       |                   |                   |
///   ==========================================================================
< ASCII >
///
/// The default precision is the number of nibbles in a machine word, and in all
/// cases indicates the minimum number of nibbles to print.
```
## Visual type:
- #table


== ./llvm/llvm_5.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libc/test/src/ctype/ispunct_test.cpp#L12-L19

```c
// Helper function to mark the sections of the ASCII table that are
// punctuation characters. These are listed below:
< ASCII >
//  Decimal    |         Symbol
//  -----------------------------------------
//  33 -  47   |  ! " $ % & ' ( ) * + , - . /
//  58 -  64   |  : ; < = > ? @
//  91 -  96   |  [ \ ] ^ _ `
// 123 - 126   |  { | } ~
< ASCII >
```
## Visual type:
- #table


== ./llvm/llvm_50.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/Support/FormatProviders.h#L246-L261

```c
/// Implementation of format_provider<T> for type `bool`
///
/// The options string of a boolean type has the grammar:
///
///   bool_options :: "" | "Y" | "y" | "D" | "d" | "T" | "t"
///
< ASCII >
///   ==================================
///   |    C    |     Meaning          |
///   ==================================
///   |    Y    |       YES / NO       |
///   |    y    |       yes / no       |
///   |  D / d  |    Integer 0 or 1    |
///   |    T    |     TRUE / FALSE     |
///   |    t    |     true / false     |
///   | (empty) |   Equivalent to 't'  |
///   ==================================
< ASCII >
```
## Visual type:
- #table


== ./llvm/llvm_51.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/Support/FormatProviders.h#L275-L296

```c
/// Implementation of format_provider<T> for floating point types.
///
/// The options string of a floating point type has the format:
///
///   float_options   :: [style][precision]
///   style           :: <see table below>
///   precision       :: <non-negative integer> 0-99
///
< ASCII >
///   =====================================================
///   |  style  |     Meaning          |      Example     |
///   -----------------------------------------------------
///   |         |                      |  Input |  Output |
///   =====================================================
///   | P / p   | Percentage           |  0.05  |  5.00%  |
///   | F / f   | Fixed point          |   1.0  |  1.00   |
///   |   E     | Exponential with E   | 100000 | 1.0E+05 |
///   |   e     | Exponential with e   | 100000 | 1.0e+05 |
///   | (empty) | Same as F / f        |        |         |
///   =====================================================
< ASCII >
///
/// The default precision is 6 for exponential (E / e) and 2 for everything
/// else.
```
## Visual type:
- #table


== ./llvm/llvm_52.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/include/llvm/Transforms/Utils/BasicBlockUtils.h#L513-L581

```c
/// Given a set of incoming and outgoing blocks, create a "hub" such that every
/// edge from an incoming block InBB to an outgoing block OutBB is now split
/// into two edges, one from InBB to the hub and another from the hub to
/// OutBB. The hub consists of a series of guard blocks, one for each outgoing
/// block. Each guard block conditionally branches to the corresponding outgoing
/// block, or the next guard block in the chain. These guard blocks are returned
/// in the argument vector.
///
/// Since the control flow edges from InBB to OutBB have now been replaced, the
/// function also updates any PHINodes in OutBB. For each such PHINode, the
/// operands corresponding to incoming blocks are moved to a new PHINode in the
/// hub, and the hub is made an operand of the original PHINode.
///
< ASCII >
/// Input CFG:
/// ----------
///
///                    Def
///                     |
///                     v
///           In1      In2
///            |        |
///            |        |
///            v        v
///  Foo ---> Out1     Out2
///                     |
///                     v
///                    Use
///
///
/// Create hub: Incoming = {In1, In2}, Outgoing = {Out1, Out2}
/// ----------------------------------------------------------
///
///             Def
///              |
///              v
///  In1        In2          Foo
///   |    Hub   |            |
///   |    + - - | - - +      |
///   |    '     v     '      V
///   +------> Guard1 -----> Out1
///        '     |     '
///        '     v     '
///        '   Guard2 -----> Out2
///        '           '      |
///        + - - - - - +      |
///                           v
///                          Use
< ASCII >
///
/// Limitations:
/// -----------
/// 1. This assumes that all terminators in the CFG are direct branches (the
///    "br" instruction). The presence of any other control flow such as
///    indirectbr, switch or callbr will cause an assert.
///
/// 2. The updates to the PHINodes are not sufficient to restore SSA
///    form. Consider a definition Def, its use Use, incoming block In2 and
///    outgoing block Out2, such that:
///    a. In2 is reachable from D or contains D.
///    b. U is reachable from Out2 or is contained in Out2.
///    c. U is not a PHINode if U is contained in Out2.
///
///    Clearly, Def dominates Out2 since the program is valid SSA. But when the
///    hub is introduced, there is a new path through the hub along which Use is
///    reachable from entry without passing through Def, and SSA is no longer
///    valid. To fix this, we need to look at all the blocks post-dominated by
///    the hub on the one hand, and dominated by Out2 on the other. This is left
///    for the caller to accomplish, since each specific use of this function
///    may have additional information which simplifies this fixup. For example,
///    see restoreSSA() in the UnifyLoopExits pass.
```
## Visual type:
- #program-graph


== ./llvm/llvm_53.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Analysis/SyncDependenceAnalysis.cpp#L1-L118

```c
//===--- SyncDependenceAnalysis.cpp - Compute Control Divergence Effects --===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// This file implements an algorithm that returns for a divergent branch
// the set of basic blocks whose phi nodes become divergent due to divergent
// control. These are the blocks that are reachable by two disjoint paths from
// the branch or loop exits that have a reaching path that is disjoint from a
// path to the loop latch.
//
// The SyncDependenceAnalysis is used in the DivergenceAnalysis to model
// control-induced divergence in phi nodes.
//
//
// -- Reference --
// The algorithm is presented in Section 5 of 
//
//   An abstract interpretation for SPMD divergence
//       on reducible control flow graphs.
//   Julian Rosemann, Simon Moll and Sebastian Hack
//   POPL '21
//
//
// -- Sync dependence --
// Sync dependence characterizes the control flow aspect of the
// propagation of branch divergence. For example,
//
//   %cond = icmp slt i32 %tid, 10
//   br i1 %cond, label %then, label %else
// then:
//   br label %merge
// else:
//   br label %merge
// merge:
//   %a = phi i32 [ 0, %then ], [ 1, %else ]
//
// Suppose %tid holds the thread ID. Although %a is not data dependent on %tid
// because %tid is not on its use-def chains, %a is sync dependent on %tid
// because the branch "br i1 %cond" depends on %tid and affects which value %a
// is assigned to.
//
//
// -- Reduction to SSA construction --
// There are two disjoint paths from A to X, if a certain variant of SSA
// construction places a phi node in X under the following set-up scheme.
//
// This variant of SSA construction ignores incoming undef values.
// That is paths from the entry without a definition do not result in
// phi nodes.
//
< ASCII >
//       entry
//     /      \
//    A        \
//  /   \       Y
// B     C     /
//  \   /  \  /
//    D     E
//     \   /
//       F
< ASCII >
//
// Assume that A contains a divergent branch. We are interested
// in the set of all blocks where each block is reachable from A
// via two disjoint paths. This would be the set {D, F} in this
// case.
// To generally reduce this query to SSA construction we introduce
// a virtual variable x and assign to x different values in each
// successor block of A.
//
< ASCII >
//           entry
//         /      \
//        A        \
//      /   \       Y
// x = 0   x = 1   /
//      \  /   \  /
//        D     E
//         \   /
//           F
//
// Our flavor of SSA construction for x will construct the following
//
//            entry
//          /      \
//         A        \
//       /   \       Y
// x0 = 0   x1 = 1  /
//       \   /   \ /
//     x2 = phi   E
//         \     /
//         x3 = phi
< ASCII >
//
// The blocks D and F contain phi nodes and are thus each reachable
// by two disjoins paths from A.
//
// -- Remarks --
// * In case of loop exits we need to check the disjoint path criterion for loops.
//   To this end, we check whether the definition of x differs between the
//   loop exit and the loop header (_after_ SSA construction).
//
// -- Known Limitations & Future Work --
// * The algorithm requires reducible loops because the implementation
//   implicitly performs a single iteration of the underlying data flow analysis.
//   This was done for pragmatism, simplicity and speed.
//
//   Relevant related work for extending the algorithm to irreducible control:
//     A simple algorithm for global data flow analysis problems.
//     Matthew S. Hecht and Jeffrey D. Ullman.
//     SIAM Journal on Computing, 4(4):519–532, December 1975.
//
// * Another reason for requiring reducible loops is that points of
//   synchronization in irreducible loops aren't 'obvious' - there is no unique
//   header where threads 'should' synchronize when entering or coming back
//   around from the latch.
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_54.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/EarlyIfConversion.cpp#L60-L80

```c
//===----------------------------------------------------------------------===//
//                                 SSAIfConv
//===----------------------------------------------------------------------===//
//
// The SSAIfConv class performs if-conversion on SSA form machine code after
// determining if it is possible. The class contains no heuristics; external
// code should be used to determine when if-conversion is a good idea.
//
< ASCII >
// SSAIfConv can convert both triangles and diamonds:
//
//   Triangle: Head              Diamond: Head
//              | \                       /  \_
//              |  \                     /    |
//              |  [TF]BB              FBB    TBB
//              |  /                     \    /
//              | /                       \  /
//             Tail                       Tail
< ASCII >
//
// Instructions in the conditional blocks TBB and/or FBB are spliced into the
// Head block, and phis in the Tail block are converted to select instructions.
//
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_55.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/IfConversion.cpp#L864-L876

```c
/// ValidForkedDiamond - Returns true if the 'true' and 'false' blocks (along
/// with their common predecessor) form a diamond if a common tail block is
/// extracted.
/// While not strictly a diamond, this pattern would form a diamond if
/// tail-merging had merged the shared tails.
< ASCII >
///           EBB
///         _/   \_
///         |     |
///        TBB   FBB
///        /  \ /   \
///  FalseBB TrueBB FalseBB
< ASCII >
/// Currently only handles analyzable branches.
/// Specifically excludes actual diamonds to avoid overlap.
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_56.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/MachineBlockPlacement.cpp#L1893-L1906

```c
// If bottom of block BB has only one successor OldTop, in most cases it is
// profitable to move it before OldTop, except the following case:
//
< ASCII >
//     -->OldTop<-
//     |    .    |
//     |    .    |
//     |    .    |
//     ---Pred   |
//          |    |
//         BB-----
< ASCII >
//
// If BB is moved before OldTop, Pred needs a taken branch to BB, and it can't
// layout the other successor below it, so it can't reduce taken branch.
// In this case we keep its original layout.
```
## Visual type:
- #custom


== ./llvm/llvm_57.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/MachineBlockPlacement.cpp#L1962-L1982

```c
// Compute the fall through gains when move NewTop before OldTop.
//
// In following diagram, edges marked as "-" are reduced fallthrough, edges
// marked as "+" are increased fallthrough, this function computes
//
< ASCII >
//      SUM(increased fallthrough) - SUM(decreased fallthrough)
//
//              |
//              | -
//              V
//        --->OldTop
//        |     .
//        |     .
//       +|     .    +
//        |   Pred --->
//        |     |-
//        |     V
//        --- NewTop <---
//              |-
//              V
< ASCII >
//
```
## Visual type:
- #custom


== ./llvm/llvm_58.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/RegisterCoalescer.cpp#L1040-L1086

```c
/// For copy B = A in BB2, if A is defined by A = B in BB0 which is a
/// predecessor of BB2, and if B is not redefined on the way from A = B
/// in BB0 to B = A in BB2, B = A in BB2 is partially redundant if the
/// execution goes through the path from BB0 to BB2. We may move B = A
/// to the predecessor without such reversed copy.
< ASCII >
/// So we will transform the program from:
///   BB0:
///      A = B;    BB1:
///       ...         ...
///     /     \      /
///             BB2:
///               ...
///               B = A;
///
/// to:
///
///   BB0:         BB1:
///      A = B;        ...
///       ...          B = A;
///     /     \       /
///             BB2:
///               ...
///
/// A special case is when BB0 and BB2 are the same BB which is the only
/// BB in a loop:
///   BB1:
///        ...
///   BB0/BB2:  ----
///        B = A;   |
///        ...      |
///        A = B;   |
///          |-------
///          |
/// We may hoist B = A from BB0/BB2 to BB1.
< ASCII >
///
/// The major preconditions for correctness to remove such partial
/// redundancy include:
/// 1. A in B = A in BB2 is defined by a PHI in BB2, and one operand of
///    the PHI is defined by the reversed copy A = B in BB0.
/// 2. No B is referenced from the start of BB2 to B = A.
/// 3. No B is defined from A = B to the end of BB0.
/// 4. BB1 has only one successor.
///
/// 2 and 4 implicitly ensure B is not live at the end of BB1.
/// 4 guarantees BB2 is hotter than BB1, so we can only move a copy to a
/// colder place, which not only prevent endless loop, but also make sure
/// the movement of copy is beneficial.
```
## Visual type:
- #program-graph


== ./llvm/llvm_59.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/RegisterCoalescer.cpp#L3194-L3232

```c
/// Consider the following situation when coalescing the copy between
/// %31 and %45 at 800. (The vertical lines represent live range segments.)
///
< ASCII >
///                              Main range         Subrange 0004 (sub2)
///                              %31    %45           %31    %45
///  544    %45 = COPY %28               +                    +
///                                      | v1                 | v1
///  560B bb.1:                          +                    +
///  624        = %45.sub2               | v2                 | v2
///  800    %31 = COPY %45        +      +             +      +
///                               | v0                 | v0
///  816    %31.sub1 = ...        +                    |
///  880    %30 = COPY %31        | v1                 +
///  928    %45 = COPY %30        |      +                    +
///                               |      | v0                 | v0  <--+
///  992B   ; backedge -> bb.1    |      +                    +        |
/// 1040        = %31.sub0        +                                    |
///                                                 This value must remain
///                                                 live-out!
< ASCII >
///
/// Assuming that %31 is coalesced into %45, the copy at 928 becomes
/// redundant, since it copies the value from %45 back into it. The
/// conflict resolution for the main range determines that %45.v0 is
/// to be erased, which is ok since %31.v1 is identical to it.
/// The problem happens with the subrange for sub2: it has to be live
/// on exit from the block, but since 928 was actually a point of
/// definition of %45.sub2, %45.sub2 was not live immediately prior
/// to that definition. As a result, when 928 was erased, the value v0
/// for %45.sub2 was pruned in pruneSubRegValues. Consequently, an
/// IMPLICIT_DEF was inserted as a "backedge" definition for %45.sub2,
/// providing an incorrect value to the use at 624.
///
/// Since the main-range values %31.v1 and %45.v0 were proved to be
/// identical, the corresponding values in subranges must also be the
/// same. A redundant copy is removed because it's not needed, and not
/// because it copied an undefined value, so any liveness that originated
/// from that copy cannot disappear. When pruning a value that started
/// at the removed copy, the corresponding identical value must be
/// extended to replace it.
```
## Visual type:
- #custom


== ./llvm/llvm_6.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libc/test/src/string/bcopy_test.cpp#L51-L54

```c
< ASCII >
// e.g. `Dst` follow `src`.
// str: [abcdefghij]
//      [__src_____]
//      [_____Dst__]
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_60.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/StackColoring.cpp#L97-L403

```c
//===----------------------------------------------------------------------===//
//                           StackColoring Pass
//===----------------------------------------------------------------------===//
//
// Stack Coloring reduces stack usage by merging stack slots when they
// can't be used together. For example, consider the following C program:
//
//     void bar(char *, int);
//     void foo(bool var) {
//         A: {
//             char z[4096];
//             bar(z, 0);
//         }
//
//         char *p;
//         char x[4096];
//         char y[4096];
//         if (var) {
//             p = x;
//         } else {
//             bar(y, 1);
//             p = y + 1024;
//         }
//     B:
//         bar(p, 2);
//     }
//
// Naively-compiled, this program would use 12k of stack space. However, the
// stack slot corresponding to `z` is always destroyed before either of the
// stack slots for `x` or `y` are used, and then `x` is only used if `var`
// is true, while `y` is only used if `var` is false. So in no time are 2
// of the stack slots used together, and therefore we can merge them,
// compiling the function using only a single 4k alloca:
//
//     void foo(bool var) { // equivalent
//         char x[4096];
//         char *p;
//         bar(x, 0);
//         if (var) {
//             p = x;
//         } else {
//             bar(x, 1);
//             p = x + 1024;
//         }
//         bar(p, 2);
//     }
//
// This is an important optimization if we want stack space to be under
// control in large functions, both open-coded ones and ones created by
// inlining.
//
// Implementation Notes:
// ---------------------
//
// An important part of the above reasoning is that `z` can't be accessed
// while the latter 2 calls to `bar` are running. This is justified because
// `z`'s lifetime is over after we exit from block `A:`, so any further
// accesses to it would be UB. The way we represent this information
// in LLVM is by having frontends delimit blocks with `lifetime.start`
// and `lifetime.end` intrinsics.
//
// The effect of these intrinsics seems to be as follows (maybe I should
// specify this in the reference?):
//
//   L1) at start, each stack-slot is marked as *out-of-scope*, unless no
//   lifetime intrinsic refers to that stack slot, in which case
//   it is marked as *in-scope*.
//   L2) on a `lifetime.start`, a stack slot is marked as *in-scope* and
//   the stack slot is overwritten with `undef`.
//   L3) on a `lifetime.end`, a stack slot is marked as *out-of-scope*.
//   L4) on function exit, all stack slots are marked as *out-of-scope*.
//   L5) `lifetime.end` is a no-op when called on a slot that is already
//   *out-of-scope*.
//   L6) memory accesses to *out-of-scope* stack slots are UB.
//   L7) when a stack-slot is marked as *out-of-scope*, all pointers to it
//   are invalidated, unless the slot is "degenerate". This is used to
//   justify not marking slots as in-use until the pointer to them is
//   used, but feels a bit hacky in the presence of things like LICM. See
//   the "Degenerate Slots" section for more details.
//
// Now, let's ground stack coloring on these rules. We'll define a slot
// as *in-use* at a (dynamic) point in execution if it either can be
// written to at that point, or if it has a live and non-undef content
// at that point.
//
// Obviously, slots that are never *in-use* together can be merged, and
// in our example `foo`, the slots for `x`, `y` and `z` are never
// in-use together (of course, sometimes slots that *are* in-use together
// might still be mergable, but we don't care about that here).
//
// In this implementation, we successively merge pairs of slots that are
// not *in-use* together. We could be smarter - for example, we could merge
// a single large slot with 2 small slots, or we could construct the
// interference graph and run a "smart" graph coloring algorithm, but with
// that aside, how do we find out whether a pair of slots might be *in-use*
// together?
//
// From our rules, we see that *out-of-scope* slots are never *in-use*,
// and from (L7) we see that "non-degenerate" slots remain non-*in-use*
// until their address is taken. Therefore, we can approximate slot activity
// using dataflow.
//
// A subtle point: naively, we might try to figure out which pairs of
// stack-slots interfere by propagating `S in-use` through the CFG for every
// stack-slot `S`, and having `S` and `T` interfere if there is a CFG point in
// which they are both *in-use*.
//
// That is sound, but overly conservative in some cases: in our (artificial)
// example `foo`, either `x` or `y` might be in use at the label `B:`, but
// as `x` is only in use if we came in from the `var` edge and `y` only
// if we came from the `!var` edge, they still can't be in use together.
// See PR32488 for an important real-life case.
//
// If we wanted to find all points of interference precisely, we could
// propagate `S in-use` and `S&T in-use` predicates through the CFG. That
// would be precise, but requires propagating `O(n^2)` dataflow facts.
//
// However, we aren't interested in the *set* of points of interference
// between 2 stack slots, only *whether* there *is* such a point. So we
// can rely on a little trick: for `S` and `T` to be in-use together,
// one of them needs to become in-use while the other is in-use (or
// they might both become in use simultaneously). We can check this
// by also keeping track of the points at which a stack slot might *start*
// being in-use.
//
// Exact first use:
// ----------------
//
// Consider the following motivating example:
//
//     int foo() {
//       char b1[1024], b2[1024];
//       if (...) {
//         char b3[1024];
//         <uses of b1, b3>;
//         return x;
//       } else {
//         char b4[1024], b5[1024];
//         <uses of b2, b4, b5>;
//         return y;
//       }
//     }
//
// In the code above, "b3" and "b4" are declared in distinct lexical
// scopes, meaning that it is easy to prove that they can share the
// same stack slot. Variables "b1" and "b2" are declared in the same
// scope, meaning that from a lexical point of view, their lifetimes
// overlap. From a control flow pointer of view, however, the two
// variables are accessed in disjoint regions of the CFG, thus it
// should be possible for them to share the same stack slot. An ideal
// stack allocation for the function above would look like:
//
//     slot 0: b1, b2
//     slot 1: b3, b4
//     slot 2: b5
//
// Achieving this allocation is tricky, however, due to the way
// lifetime markers are inserted. Here is a simplified view of the
// control flow graph for the code above:
//
< ASCII >
//                +------  block 0 -------+
//               0| LIFETIME_START b1, b2 |
//               1| <test 'if' condition> |
//                +-----------------------+
//                   ./              \.
//   +------  block 1 -------+   +------  block 2 -------+
//  2| LIFETIME_START b3     |  5| LIFETIME_START b4, b5 |
//  3| <uses of b1, b3>      |  6| <uses of b2, b4, b5>  |
//  4| LIFETIME_END b3       |  7| LIFETIME_END b4, b5   |
//   +-----------------------+   +-----------------------+
//                   \.              /.
//                +------  block 3 -------+
//               8| <cleanupcode>         |
//               9| LIFETIME_END b1, b2   |
//              10| return                |
//                +-----------------------+
< ASCII >
//
// If we create live intervals for the variables above strictly based
// on the lifetime markers, we'll get the set of intervals on the
// left. If we ignore the lifetime start markers and instead treat a
// variable's lifetime as beginning with the first reference to the
// var, then we get the intervals on the right.
//
//            LIFETIME_START      First Use
//     b1:    [0,9]               [3,4] [8,9]
//     b2:    [0,9]               [6,9]
//     b3:    [2,4]               [3,4]
//     b4:    [5,7]               [6,7]
//     b5:    [5,7]               [6,7]
//
// For the intervals on the left, the best we can do is overlap two
// variables (b3 and b4, for example); this gives us a stack size of
// 4*1024 bytes, not ideal. When treating first-use as the start of a
// lifetime, we can additionally overlap b1 and b5, giving us a 3*1024
// byte stack (better).
//
// Degenerate Slots:
// -----------------
//
// Relying entirely on first-use of stack slots is problematic,
// however, due to the fact that optimizations can sometimes migrate
// uses of a variable outside of its lifetime start/end region. Here
// is an example:
//
//     int bar() {
//       char b1[1024], b2[1024];
//       if (...) {
//         <uses of b2>
//         return y;
//       } else {
//         <uses of b1>
//         while (...) {
//           char b3[1024];
//           <uses of b3>
//         }
//       }
//     }
//
// Before optimization, the control flow graph for the code above
// might look like the following:
//
< ASCII >
//                +------  block 0 -------+
//               0| LIFETIME_START b1, b2 |
//               1| <test 'if' condition> |
//                +-----------------------+
//                   ./              \.
//   +------  block 1 -------+    +------- block 2 -------+
//  2| <uses of b2>          |   3| <uses of b1>          |
//   +-----------------------+    +-----------------------+
//              |                            |
//              |                 +------- block 3 -------+ <-\.
//              |                4| <while condition>     |    |
//              |                 +-----------------------+    |
//              |               /          |                   |
//              |              /  +------- block 4 -------+
//              \             /  5| LIFETIME_START b3     |    |
//               \           /   6| <uses of b3>          |    |
//                \         /    7| LIFETIME_END b3       |    |
//                 \        |    +------------------------+    |
//                  \       |                 \                /
//                +------  block 5 -----+      \---------------
//               8| <cleanupcode>       |
//               9| LIFETIME_END b1, b2 |
//              10| return              |
//                +---------------------+
< ASCII >
//
// During optimization, however, it can happen that an instruction
// computing an address in "b3" (for example, a loop-invariant GEP) is
// hoisted up out of the loop from block 4 to block 2.  [Note that
// this is not an actual load from the stack, only an instruction that
// computes the address to be loaded]. If this happens, there is now a
// path leading from the first use of b3 to the return instruction
// that does not encounter the b3 LIFETIME_END, hence b3's lifetime is
// now larger than if we were computing live intervals strictly based
// on lifetime markers. In the example above, this lengthened lifetime
// would mean that it would appear illegal to overlap b3 with b2.
//
// To deal with this such cases, the code in ::collectMarkers() below
// tries to identify "degenerate" slots -- those slots where on a single
// forward pass through the CFG we encounter a first reference to slot
// K before we hit the slot K lifetime start marker. For such slots,
// we fall back on using the lifetime start marker as the beginning of
// the variable's lifetime.  NB: with this implementation, slots can
// appear degenerate in cases where there is unstructured control flow:
//
//    if (q) goto mid;
//    if (x > 9) {
//         int b[100];
//         memcpy(&b[0], ...);
//    mid: b[k] = ...;
//         abc(&b);
//    }
//
// If in RPO ordering chosen to walk the CFG  we happen to visit the b[k]
// before visiting the memcpy block (which will contain the lifetime start
// for "b" then it will appear that 'b' has a degenerate lifetime.
//
// Handle Windows Exception with LifetimeStartOnFirstUse:
// -----------------
//
// There was a bug for using LifetimeStartOnFirstUse in win32.
// class Type1 {
// ...
// ~Type1(){ write memory;}
// }
// ...
// try{
// Type1 V
// ...
// } catch (Type2 X){
// ...
// }
// For variable X in catch(X), we put point pX=&(&X) into ConservativeSlots
// to prevent using LifetimeStartOnFirstUse. Because pX may merged with
// object V which may call destructor after implicitly writing pX. All these
// are done in C++ EH runtime libs (through CxxThrowException), and can't
// obviously check it in IR level.
//
// The loader of pX, without obvious writing IR, is usually the first LOAD MI
// in EHPad, Some like:
// bb.x.catch.i (landing-pad, ehfunclet-entry):
// ; predecessors: %bb...
//   successors: %bb...
//  %n:gr32 = MOV32rm %stack.pX ...
//  ...
// The Type2** %stack.pX will only be written in EH runtime libs, so we
// check the StoreSlots to screen it out.
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_61.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/LiveDebugValues/InstrRefBasedImpl.h#L584-L620

```c
/// Tracker for what values are in machine locations. Listens to the Things
/// being Done by various instructions, and maintains a table of what machine
/// locations have what values (as defined by a ValueIDNum).
///
/// There are potentially a much larger number of machine locations on the
/// target machine than the actual working-set size of the function. On x86 for
/// example, we're extremely unlikely to want to track values through control
/// or debug registers. To avoid doing so, MLocTracker has several layers of
/// indirection going on, described below, to avoid unnecessarily tracking
/// any location.
///
< ASCII >
/// Here's a sort of diagram of the indexes, read from the bottom up:
///
///           Size on stack   Offset on stack
///                 \              /
///          Stack Idx (Where in slot is this?)
///                         /
///                        /
/// Slot Num (%stack.0)   /
/// FrameIdx => SpillNum /
///              \      /
///           SpillID (int)              Register number (int)
///                      \                  /
///                      LocationID => LocIdx
///                                |
///                       LocIdx => ValueIDNum
< ASCII >
///
/// The aim here is that the LocIdx => ValueIDNum vector is just an array of
/// values in numbered locations, so that later analyses can ignore whether the
/// location is a register or otherwise. To map a register / spill location to
/// a LocIdx, you have to use the (sparse) LocationID => LocIdx map. And to
/// build a LocationID for a stack slot, you need to combine identifiers for
/// which stack slot it is and where within that slot is being described.
///
/// Register mask operands cause trouble by technically defining every register;
/// various hacks are used to avoid tracking registers that are never read and
/// only written by regmasks.
```
## Visual type:
- #custom


== ./llvm/llvm_62.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/SelectionDAG/DAGCombiner.cpp#L3137-L3158

```c
/**
 * If we are facing some sort of diamond carry propapagtion pattern try to
 * break it up to generate something like:
 *   (addcarry X, 0, (addcarry A, B, Z):Carry)
 *
 * The end result is usually an increase in operation required, but because the
 * carry is now linearized, other transforms can kick in and optimize the DAG.
 *
< ASCII >
 * Patterns typically look something like
 *            (uaddo A, B)
 *             /       \
 *          Carry      Sum
 *            |          \
 *            | (addcarry *, 0, Z)
 *            |       /
 *             \   Carry
 *              |   /
 * (addcarry X, *, *)
< ASCII >
 *
 * But numerous variation exist. Our goal is to identify A, B, X and Z and
 * produce a combine with a single path for carry propagation.
 */
```
## Visual type:
- #program-graph


== ./llvm/llvm_63.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/SelectionDAG/DAGCombiner.cpp#L3224-L3249

```c
// If we are facing some sort of diamond carry/borrow in/out pattern try to
// match patterns like:
//
< ASCII >
//          (uaddo A, B)            CarryIn
//            |  \                     |
//            |   \                    |
//    PartialSum   PartialCarryOutX   /
//            |        |             /
//            |    ____|____________/
//            |   /    |
//     (uaddo *, *)    \________
//       |  \                   \
//       |   \                   |
//       |    PartialCarryOutY   |
//       |        \              |
//       |         \            /
//   AddCarrySum    |    ______/
//                  |   /
//   CarryOut = (or *, *)
< ASCII >
//
// And generate ADDCARRY (or SUBCARRY) with two result values:
//
//    {AddCarrySum, CarryOut} = (addcarry A, B, CarryIn)
//
// Our goal is to identify A, B, and CarryIn and produce ADDCARRY/SUBCARRY with
// a single path for carry/borrow out propagation:
```
## Visual type:
- #program-graph


== ./llvm/llvm_64.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/SelectionDAG/DAGCombiner.cpp#L7966-L7997

```c
/// However, there is a special case when dealing with vector loads -- we allow
/// more than one use if the load is a vector type.  Since the values that
/// contribute to the byte ultimately come from the ExtractVectorElements of the
/// Load, we don't care if the Load has uses other than ExtractVectorElements,
/// because those operations are independent from the pattern to be combined.
/// For vector loads, we simply care that the ByteProviders are adjacent
/// positions of the same vector, and their index matches the byte that is being
/// provided. This is captured by the \p VectorIndex algorithm. \p VectorIndex
/// is the index used in an ExtractVectorElement, and \p StartingIndex is the
/// byte position we are trying to provide for the LoadCombine. If these do
/// not match, then we can not combine the vector loads. \p Index uses the
/// byte position we are trying to provide for and is matched against the
/// shl and load size. The \p Index algorithm ensures the requested byte is
/// provided for by the pattern, and the pattern does not over provide bytes.
///
///
< ASCII >
/// The supported LoadCombine pattern for vector loads is as follows
///                              or
///                          /        \
///                         or        shl
///                       /     \      |
///                     or      shl   zext
///                   /    \     |     |
///                 shl   zext  zext  EVE*
///                  |     |     |     |
///                 zext  EVE*  EVE*  LOAD
///                  |     |     |
///                 EVE*  LOAD  LOAD
///                  |
///                 LOAD
< ASCII >
///
/// *ExtractVectorElement
```
## Visual type:
- #tree


== ./llvm/llvm_65.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/CodeGen/SelectionDAG/ScheduleDAGRRList.cpp#L2934-L2963

```c
/// PrescheduleNodesWithMultipleUses - Nodes with multiple uses
/// are not handled well by the general register pressure reduction
/// heuristics. When presented with code like this:
///
< ASCII >
///      N
///    / |
///   /  |
///  U  store
///  |
/// ...
< ASCII >
///
/// the heuristics tend to push the store up, but since the
/// operand of the store has another use (U), this would increase
/// the length of that other use (the U->N edge).
///
/// This function transforms code like the above to route U's
/// dependence through the store when possible, like this:
///
< ASCII >
///      N
///      ||
///      ||
///     store
///       |
///       U
///       |
///      ...
< ASCII >
///
/// This results in the store being scheduled immediately
/// after N, which shortens the U->N live range, reducing
/// register pressure.
```
## Visual type:
- #program-graph


== ./llvm/llvm_66.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/IR/SafepointIRVerifier.cpp#L397-L452

```c
/// Builds BasicBlockState for each BB of the function.
/// It can traverse function for verification and provides all required
/// information.
///
/// GC pointer may be in one of three states: relocated, unrelocated and
/// poisoned.
/// Relocated pointer may be used without any restrictions.
/// Unrelocated pointer cannot be dereferenced, passed as argument to any call
/// or returned. Unrelocated pointer may be safely compared against another
/// unrelocated pointer or against a pointer exclusively derived from null.
/// Poisoned pointers are produced when we somehow derive pointer from relocated
/// and unrelocated pointers (e.g. phi, select). This pointers may be safely
/// used in a very limited number of situations. Currently the only way to use
/// it is comparison against constant exclusively derived from null. All
/// limitations arise due to their undefined state: this pointers should be
/// treated as relocated and unrelocated simultaneously.
/// Rules of deriving:
/// R + U = P - that's where the poisoned pointers come from
/// P + X = P
/// U + U = U
/// R + R = R
/// X + C = X
/// Where "+" - any operation that somehow derive pointer, U - unrelocated,
/// R - relocated and P - poisoned, C - constant, X - U or R or P or C or
/// nothing (in case when "+" is unary operation).
/// Deriving of pointers by itself is always safe.
/// NOTE: when we are making decision on the status of instruction's result:
/// a) for phi we need to check status of each input *at the end of
///    corresponding predecessor BB*.
/// b) for other instructions we need to check status of each input *at the
///    current point*.
///
< ASCII >
/// FIXME: This works fairly well except one case
///     bb1:
///     p = *some GC-ptr def*
///     p1 = gep p, offset
///         /     |
///        /      |
///    bb2:       |
///    safepoint  |
///        \      |
///         \     |
///      bb3:
///      p2 = phi [p, bb2] [p1, bb1]
///      p3 = phi [p, bb2] [p, bb1]
///      here p and p1 is unrelocated
///           p2 and p3 is poisoned (though they shouldn't be)
< ASCII >
///
/// This leads to some weird results:
///      cmp eq p, p2 - illegal instruction (false-positive)
///      cmp eq p1, p2 - illegal instruction (false-positive)
///      cmp eq p, p3 - illegal instruction (false-positive)
///      cmp eq p, p1 - ok
/// To fix this we need to introduce conception of generations and be able to
/// check if two values belong to one generation or not. This way p2 will be
/// considered to be unrelocated and no false alarm will happen.
```
## Visual type:
- #program-graph


== ./llvm/llvm_67.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Support/BLAKE3/blake3.c#L1-L7

```c
< ASCII >
/*===-- blake3.c - BLAKE3 C Implementation ------------------------*- C -*-===*\
|*                                                                            *|
|* Released into the public domain with CC0 1.0                               *|
|* See 'llvm/lib/Support/BLAKE3/LICENSE' for info.                            *|
|* SPDX-License-Identifier: CC0-1.0                                           *|
|*                                                                            *|
\*===----------------------------------------------------------------------===*/
< ASCII >
```
## Visual type:



== ./llvm/llvm_68.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/AArch64/AArch64ConditionalCompares.cpp#L71-L134

```c
//===----------------------------------------------------------------------===//
//                                 SSACCmpConv
//===----------------------------------------------------------------------===//
//
// The SSACCmpConv class performs ccmp-conversion on SSA form machine code
// after determining if it is possible. The class contains no heuristics;
// external code should be used to determine when ccmp-conversion is a good
// idea.
//
// CCmp-formation works on a CFG representing chained conditions, typically
// from C's short-circuit || and && operators:
//
< ASCII >
//   From:         Head            To:         Head
//                 / |                         CmpBB
//                /  |                         / |
//               |  CmpBB                     /  |
//               |  / |                    Tail  |
//               | /  |                      |   |
//              Tail  |                      |   |
//                |   |                      |   |
//               ... ...                    ... ...
< ASCII >
< ASCII >
//
// The Head block is terminated by a br.cond instruction, and the CmpBB block
// contains compare + br.cond. Tail must be a successor of both.
//
// The cmp-conversion turns the compare instruction in CmpBB into a conditional
// compare, and merges CmpBB into Head, speculatively executing its
// instructions. The AArch64 conditional compare instructions have an immediate
// operand that specifies the NZCV flag values when the condition is false and
// the compare isn't executed. This makes it possible to chain compares with
// different condition codes.
//
// Example:
//
//    if (a == 5 || b == 17)
//      foo();
//
//    Head:
//       cmp  w0, #5
//       b.eq Tail
//    CmpBB:
//       cmp  w1, #17
//       b.eq Tail
//    ...
//    Tail:
//      bl _foo
//
//  Becomes:
//
//    Head:
//       cmp  w0, #5
//       ccmp w1, #17, 4, ne  ; 4 = nZcv
//       b.eq Tail
//    ...
//    Tail:
//      bl _foo
//
// The ccmp condition code is the one that would cause the Head terminator to
// branch to CmpBB.
//
// FIXME: It should also be possible to speculate a block on the critical edge
// between Head and Tail, just like if-converting a diamond.
//
// FIXME: Handle PHIs in Tail by turning them into selects (if-conversion).
```
## Visual type:
- #control-flow-graph


== ./llvm/llvm_69.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/AArch64/AArch64ConditionOptimizer.cpp#L1-L58

```c
//=- AArch64ConditionOptimizer.cpp - Remove useless comparisons for AArch64 -=//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// This pass tries to make consecutive compares of values use same operands to
// allow CSE pass to remove duplicated instructions.  For this it analyzes
// branches and adjusts comparisons with immediate values by converting:
//  * GE -> GT
//  * GT -> GE
//  * LT -> LE
//  * LE -> LT
// and adjusting immediate values appropriately.  It basically corrects two
// immediate values towards each other to make them equal.
//
// Consider the following example in C:
//
< ASCII >
//   if ((a < 5 && ...) || (a > 5 && ...)) {
//        ~~~~~             ~~~~~
//          ^                 ^
//          x                 y
< ASCII >
//
// Here both "x" and "y" expressions compare "a" with "5".  When "x" evaluates
// to "false", "y" can just check flags set by the first comparison.  As a
// result of the canonicalization employed by
// SelectionDAGBuilder::visitSwitchCase, DAGCombine, and other target-specific
// code, assembly ends up in the form that is not CSE friendly:
//
//     ...
//     cmp      w8, #4
//     b.gt     .LBB0_3
//     ...
//   .LBB0_3:
//     cmp      w8, #6
//     b.lt     .LBB0_6
//     ...
//
// Same assembly after the pass:
//
< ASCII >
//     ...
//     cmp      w8, #5
//     b.ge     .LBB0_3
//     ...
//   .LBB0_3:
//     cmp      w8, #5     // <-- CSE pass removes this instruction
//     b.le     .LBB0_6
//     ...
< ASCII >
//
// Currently only SUBS and ADDS followed by b.?? are supported.
//
// TODO: maybe handle TBNZ/TBZ the same way as CMP when used instead for "a < 0"
// TODO: handle other conditional instructions (e.g. CSET)
// TODO: allow second branching to be anything if it doesn't require adjusting
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #annotation


== ./llvm/llvm_7.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libc/test/src/string/memmove_test.cpp#L55-L58

```c
// e.g. `Dst` follow `src`.
< ASCII >
// str: [abcdefghij]
//      [__src_____]
//      [_____Dst__]
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_70.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/AArch64/AArch64FrameLowering.cpp#L1-L186

```c
//===- AArch64FrameLowering.cpp - AArch64 Frame Lowering -------*- C++ -*-====//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// This file contains the AArch64 implementation of TargetFrameLowering class.
//
// On AArch64, stack frames are structured as follows:
//
// The stack grows downward.
//
// All of the individual frame areas on the frame below are optional, i.e. it's
// possible to create a function so that the particular area isn't present
// in the frame.
//
< ASCII >
// At function entry, the "frame" looks as follows:
//
// |                                   | Higher address
// |-----------------------------------|
// |                                   |
// | arguments passed on the stack     |
// |                                   |
// |-----------------------------------| <- sp
// |                                   | Lower address
< ASCII >
//
//
// After the prologue has run, the frame has the following general structure.
// Note that this doesn't depict the case where a red-zone is used. Also,
// technically the last frame area (VLAs) doesn't get created until in the
// main function body, after the prologue is run. However, it's depicted here
// for completeness.
//
< ASCII >
// |                                   | Higher address
// |-----------------------------------|
// |                                   |
// | arguments passed on the stack     |
// |                                   |
// |-----------------------------------|
// |                                   |
// | (Win64 only) varargs from reg     |
// |                                   |
// |-----------------------------------|
// |                                   |
// | callee-saved gpr registers        | <--.
// |                                   |    | On Darwin platforms these
// |- - - - - - - - - - - - - - - - - -|    | callee saves are swapped,
// | prev_lr                           |    | (frame record first)
// | prev_fp                           | <--'
// | async context if needed           |
// | (a.k.a. "frame record")           |
// |-----------------------------------| <- fp(=x29)
// |                                   |
// | callee-saved fp/simd/SVE regs     |
// |                                   |
// |-----------------------------------|
// |                                   |
// |        SVE stack objects          |
// |                                   |
// |-----------------------------------|
// |.empty.space.to.make.part.below....|
// |.aligned.in.case.it.needs.more.than| (size of this area is unknown at
// |.the.standard.16-byte.alignment....|  compile time; if present)
// |-----------------------------------|
// |                                   |
// | local variables of fixed size     |
// | including spill slots             |
// |-----------------------------------| <- bp(not defined by ABI,
// |.variable-sized.local.variables....|       LLVM chooses X19)
// |.(VLAs)............................| (size of this area is unknown at
// |...................................|  compile time)
// |-----------------------------------| <- sp
// |                                   | Lower address
< ASCII >
//
//
// To access the data in a frame, at-compile time, a constant offset must be
// computable from one of the pointers (fp, bp, sp) to access it. The size
// of the areas with a dotted background cannot be computed at compile-time
// if they are present, making it required to have all three of fp, bp and
// sp to be set up to be able to access all contents in the frame areas,
// assuming all of the frame areas are non-empty.
//
// For most functions, some of the frame areas are empty. For those functions,
// it may not be necessary to set up fp or bp:
// * A base pointer is definitely needed when there are both VLAs and local
//   variables with more-than-default alignment requirements.
// * A frame pointer is definitely needed when there are local variables with
//   more-than-default alignment requirements.
//
// For Darwin platforms the frame-record (fp, lr) is stored at the top of the
// callee-saved area, since the unwind encoding does not allow for encoding
// this dynamically and existing tools depend on this layout. For other
// platforms, the frame-record is stored at the bottom of the (gpr) callee-saved
// area to allow SVE stack objects (allocated directly below the callee-saves,
// if available) to be accessed directly from the framepointer.
// The SVE spill/fill instructions have VL-scaled addressing modes such
// as:
//    ldr z8, [fp, #-7 mul vl]
// For SVE the size of the vector length (VL) is not known at compile-time, so
// '#-7 mul vl' is an offset that can only be evaluated at runtime. With this
// layout, we don't need to add an unscaled offset to the framepointer before
// accessing the SVE object in the frame.
//
// In some cases when a base pointer is not strictly needed, it is generated
// anyway when offsets from the frame pointer to access local variables become
// so large that the offset can't be encoded in the immediate fields of loads
// or stores.
//
// Outgoing function arguments must be at the bottom of the stack frame when
// calling another function. If we do not have variable-sized stack objects, we
// can allocate a "reserved call frame" area at the bottom of the local
// variable area, large enough for all outgoing calls. If we do have VLAs, then
// the stack pointer must be decremented and incremented around each call to
// make space for the arguments below the VLAs.
//
// FIXME: also explain the redzone concept.
//
// An example of the prologue:
//
//     .globl __foo
//     .align 2
//  __foo:
// Ltmp0:
//     .cfi_startproc
//     .cfi_personality 155, ___gxx_personality_v0
// Leh_func_begin:
//     .cfi_lsda 16, Lexception33
//
//     stp  xa,bx, [sp, -#offset]!
//     ...
//     stp  x28, x27, [sp, #offset-32]
//     stp  fp, lr, [sp, #offset-16]
//     add  fp, sp, #offset - 16
//     sub  sp, sp, #1360
//
< ASCII >
// The Stack:
//       +-------------------------------------------+
// 10000 | ........ | ........ | ........ | ........ |
// 10004 | ........ | ........ | ........ | ........ |
//       +-------------------------------------------+
// 10008 | ........ | ........ | ........ | ........ |
// 1000c | ........ | ........ | ........ | ........ |
//       +===========================================+
// 10010 |                X28 Register               |
// 10014 |                X28 Register               |
//       +-------------------------------------------+
// 10018 |                X27 Register               |
// 1001c |                X27 Register               |
//       +===========================================+
// 10020 |                Frame Pointer              |
// 10024 |                Frame Pointer              |
//       +-------------------------------------------+
// 10028 |                Link Register              |
// 1002c |                Link Register              |
//       +===========================================+
// 10030 | ........ | ........ | ........ | ........ |
// 10034 | ........ | ........ | ........ | ........ |
//       +-------------------------------------------+
// 10038 | ........ | ........ | ........ | ........ |
// 1003c | ........ | ........ | ........ | ........ |
//       +-------------------------------------------+
< ASCII >
//
//     [sp] = 10030        ::    >>initial value<<
//     sp = 10020          ::  stp fp, lr, [sp, #-16]!
//     fp = sp == 10020    ::  mov fp, sp
//     [sp] == 10020       ::  stp x28, x27, [sp, #-16]!
//     sp == 10010         ::    >>final value<<
//
// The frame pointer (w29) points to address 10020. If we use an offset of
// '16' from 'w29', we get the CFI offsets of -8 for w30, -16 for w29, -24
// for w27, and -32 for w28:
//
//  Ltmp1:
//     .cfi_def_cfa w29, 16
//  Ltmp2:
//     .cfi_offset w30, -8
//  Ltmp3:
//     .cfi_offset w29, -16
//  Ltmp4:
//     .cfi_offset w27, -24
//  Ltmp5:
//     .cfi_offset w28, -32
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #memory-layout


== ./llvm/llvm_71.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/AArch64/AArch64InstrFormats.td#L11327-L11334

```c
// Instruction encodings:
//
< ASCII >
//      31 30|29  24|23|22|21|20 16|15|14  10|9 5|4 0
// CAS  SZ   |001000|1 |A |1 |Rs   |R |11111 |Rn |Rt
// CASP  0|SZ|001000|0 |A |1 |Rs   |R |11111 |Rn |Rt
// SWP  SZ   |111000|A |R |1 |Rs   |1 |OPC|00|Rn |Rt
// LD   SZ   |111000|A |R |1 |Rs   |0 |OPC|00|Rn |Rt
// ST   SZ   |111000|A |R |1 |Rs   |0 |OPC|00|Rn |11111
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_72.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/AArch64/AArch64ISelLowering.cpp#L19825-L19884

```c
// This function does a whole lot of voodoo to determine if the tests are
// equivalent without and with a mask. Essentially what happens is that given a
// DAG resembling:
//
< ASCII >
//  +-------------+ +-------------+ +-------------+ +-------------+
//  |    Input    | | AddConstant | | CompConstant| |     CC      |
//  +-------------+ +-------------+ +-------------+ +-------------+
//           |           |           |               |
//           V           V           |    +----------+
//          +-------------+  +----+  |    |
//          |     ADD     |  |0xff|  |    |
//          +-------------+  +----+  |    |
//                  |           |    |    |
//                  V           V    |    |
//                 +-------------+   |    |
//                 |     AND     |   |    |
//                 +-------------+   |    |
//                      |            |    |
//                      +-----+      |    |
//                            |      |    |
//                            V      V    V
//                           +-------------+
//                           |     CMP     |
//                           +-------------+
< ASCII >
//
// The AND node may be safely removed for some combinations of inputs. In
// particular we need to take into account the extension type of the Input,
// the exact values of AddConstant, CompConstant, and CC, along with the nominal
// width of the input (this can work for any width inputs, the above graph is
// specific to 8 bits.
//
// The specific equations were worked out by generating output tables for each
// AArch64CC value in terms of and AddConstant (w1), CompConstant(w2). The
// problem was simplified by working with 4 bit inputs, which means we only
// needed to reason about 24 distinct bit patterns: 8 patterns unique to zero
// extension (8,15), 8 patterns unique to sign extensions (-8,-1), and 8
// patterns present in both extensions (0,7). For every distinct set of
// AddConstant and CompConstants bit patterns we can consider the masked and
// unmasked versions to be equivalent if the result of this function is true for
// all 16 distinct bit patterns of for the current extension type of Input (w0).
//
//   sub      w8, w0, w1
//   and      w10, w8, #0x0f
//   cmp      w8, w2
//   cset     w9, AArch64CC
//   cmp      w10, w2
//   cset     w11, AArch64CC
//   cmp      w9, w11
//   cset     w0, eq
//   ret
//
// Since the above function shows when the outputs are equivalent it defines
// when it is safe to remove the AND. Unfortunately it only runs on AArch64 and
// would be expensive to run during compiles. The equations below were written
// in a test harness that confirmed they gave equivalent outputs to the above
// for all inputs function, so they can be used determine if the removal is
// legal instead.
//
// isEquivalentMaskless() is the code for testing if the AND can be removed
// factored out of the DAG recognition as the DAG can take several forms.
```
## Visual type:
- #program-graph


== ./llvm/llvm_73.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/AArch64/AArch64SVEInstrInfo.td#L13-L26

```c
// For predicated nodes where the entire operation is controlled by a governing
// predicate, please stick to a similar naming convention as used for the
// ISD nodes:
//
< ASCII >
//    SDNode      <=>     AArch64ISD
//    -------------------------------
//    _m<n>       <=>     _MERGE_OP<n>
//    _mt         <=>     _MERGE_PASSTHRU
//    _z          <=>     _MERGE_ZERO
//    _p          <=>     _PRED
< ASCII >
//
//  Given the context of this file, it is not strictly necessary to use _p to
//  distinguish predicated from unpredicated nodes given that most SVE
//  instructions are predicated.
```
## Visual type:
- #table


== ./llvm/llvm_74.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/AMDGPU/SIInstrInfo.cpp#L7900-L7929

```c
// Depending on the used address space and instructions, some immediate offsets
// are allowed and some are not.
// In general, flat instruction offsets can only be non-negative, global and
// scratch instruction offsets can also be negative.
//
// There are several bugs related to these offsets:
// On gfx10.1, flat instructions that go into the global address space cannot
// use an offset.
//
// For scratch instructions, the address can be either an SGPR or a VGPR.
// The following offsets can be used, depending on the architecture (x means
// cannot be used):
< ASCII >
// +----------------------------+------+------+
// | Address-Mode               | SGPR | VGPR |
// +----------------------------+------+------+
// | gfx9                       |      |      |
// | negative, 4-aligned offset | x    | ok   |
// | negative, unaligned offset | x    | ok   |
// +----------------------------+------+------+
// | gfx10                      |      |      |
// | negative, 4-aligned offset | ok   | ok   |
// | negative, unaligned offset | ok   | x    |
// +----------------------------+------+------+
// | gfx10.3                    |      |      |
// | negative, 4-aligned offset | ok   | ok   |
// | negative, unaligned offset | ok   | ok   |
// +----------------------------+------+------+
< ASCII >
//
// This function ignores the addressing mode, so if an offset cannot be used in
// one addressing mode, it is considered illegal.
```
## Visual type:
- #table


== ./llvm/llvm_75.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/AMDGPU/Utils/AMDGPUBaseInfo.h#L608-L637

```c
// Interface functions of this class map VOPD component operand indices
// to indices of operands in MachineInstr/MCInst or parsed operands array.
//
// Note that this class operates with 3 kinds of indices:
// - VOPD component operand indices (Component::DST, Component::SRC0, etc.);
// - MC operand indices (they refer operands in a MachineInstr/MCInst);
// - parsed operand indices (they refer operands in parsed operands array).
//
// For SINGLE components mapping between these indices is trivial.
// But things get more complicated for COMPONENT_X and
// COMPONENT_Y because these components share the same
// MachineInstr/MCInst and the same parsed operands array.
// Below is an example of component operand to parsed operand
// mapping for the following instruction:
//
//   v_dual_add_f32 v255, v4, v5 :: v_dual_mov_b32 v6, v1
//
< ASCII >
//                          PARSED        COMPONENT         PARSED
// COMPONENT               OPERANDS     OPERAND INDEX    OPERAND INDEX
// -------------------------------------------------------------------
//                     "v_dual_add_f32"                        0
// v_dual_add_f32            v255          0 (DST)    -->      1
//                           v4            1 (SRC0)   -->      2
//                           v5            2 (SRC1)   -->      3
//                          "::"                               4
//                     "v_dual_mov_b32"                        5
// v_dual_mov_b32            v6            0 (DST)    -->      6
//                           v1            1 (SRC0)   -->      7
// -------------------------------------------------------------------
< ASCII >
//
```
## Visual type:
- #table


== ./llvm/llvm_76.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/ARM/ARMBaseInstrInfo.cpp#L5677-L5777

```c
/// Constants defining how certain sequences should be outlined.
/// This encompasses how an outlined function should be called, and what kind of
/// frame should be emitted for that outlined function.
///
/// \p MachineOutlinerTailCall implies that the function is being created from
/// a sequence of instructions ending in a return.
///
/// That is,
///
/// I1                                OUTLINED_FUNCTION:
/// I2    --> B OUTLINED_FUNCTION     I1
/// BX LR                             I2
///                                   BX LR
///
< ASCII >
/// +-------------------------+--------+-----+
/// |                         | Thumb2 | ARM |
/// +-------------------------+--------+-----+
/// | Call overhead in Bytes  |      4 |   4 |
/// | Frame overhead in Bytes |      0 |   0 |
/// | Stack fixup required    |     No |  No |
/// +-------------------------+--------+-----+
< ASCII >
///
/// \p MachineOutlinerThunk implies that the function is being created from
/// a sequence of instructions ending in a call. The outlined function is
/// called with a BL instruction, and the outlined function tail-calls the
/// original call destination.
///
/// That is,
///
/// I1                                OUTLINED_FUNCTION:
/// I2   --> BL OUTLINED_FUNCTION     I1
/// BL f                              I2
///                                   B f
///
< ASCII >
/// +-------------------------+--------+-----+
/// |                         | Thumb2 | ARM |
/// +-------------------------+--------+-----+
/// | Call overhead in Bytes  |      4 |   4 |
/// | Frame overhead in Bytes |      0 |   0 |
/// | Stack fixup required    |     No |  No |
/// +-------------------------+--------+-----+
< ASCII >
///
/// \p MachineOutlinerNoLRSave implies that the function should be called using
/// a BL instruction, but doesn't require LR to be saved and restored. This
/// happens when LR is known to be dead.
///
/// That is,
///
< ASCII >
/// I1                                OUTLINED_FUNCTION:
/// I2 --> BL OUTLINED_FUNCTION       I1
/// I3                                I2
///                                   I3
///                                   BX LR
///
/// +-------------------------+--------+-----+
/// |                         | Thumb2 | ARM |
/// +-------------------------+--------+-----+
/// | Call overhead in Bytes  |      4 |   4 |
/// | Frame overhead in Bytes |      2 |   4 |
/// | Stack fixup required    |     No |  No |
/// +-------------------------+--------+-----+
< ASCII >
///
/// \p MachineOutlinerRegSave implies that the function should be called with a
/// save and restore of LR to an available register. This allows us to avoid
/// stack fixups. Note that this outlining variant is compatible with the
/// NoLRSave case.
///
/// That is,
///
< ASCII >
/// I1     Save LR                    OUTLINED_FUNCTION:
/// I2 --> BL OUTLINED_FUNCTION       I1
/// I3     Restore LR                 I2
///                                   I3
///                                   BX LR
///
/// +-------------------------+--------+-----+
/// |                         | Thumb2 | ARM |
/// +-------------------------+--------+-----+
/// | Call overhead in Bytes  |      8 |  12 |
/// | Frame overhead in Bytes |      2 |   4 |
/// | Stack fixup required    |     No |  No |
/// +-------------------------+--------+-----+
< ASCII >
///
/// \p MachineOutlinerDefault implies that the function should be called with
/// a save and restore of LR to the stack.
///
/// That is,
///
< ASCII >
/// I1     Save LR                    OUTLINED_FUNCTION:
/// I2 --> BL OUTLINED_FUNCTION       I1
/// I3     Restore LR                 I2
///                                   I3
///                                   BX LR
///
/// +-------------------------+--------+-----+
/// |                         | Thumb2 | ARM |
/// +-------------------------+--------+-----+
/// | Call overhead in Bytes  |      8 |  12 |
/// | Frame overhead in Bytes |      2 |   4 |
/// | Stack fixup required    |    Yes | Yes |
/// +-------------------------+--------+-----+
< ASCII >
```
## Visual type:
- #table


== ./llvm/llvm_77.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/ARM/ARMFrameLowering.cpp#L1-L108

```c
//===- ARMFrameLowering.cpp - ARM Frame Information -----------------------===//
//
// Part of the LLVM Project, under the Apache License v2.0 with LLVM Exceptions.
// See https://llvm.org/LICENSE.txt for license information.
// SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception
//
//===----------------------------------------------------------------------===//
//
// This file contains the ARM implementation of TargetFrameLowering class.
//
//===----------------------------------------------------------------------===//
//
// This file contains the ARM implementation of TargetFrameLowering class.
//
// On ARM, stack frames are structured as follows:
//
// The stack grows downward.
//
// All of the individual frame areas on the frame below are optional, i.e. it's
// possible to create a function so that the particular area isn't present
// in the frame.
//
// At function entry, the "frame" looks as follows:
//
< ASCII >
// |                                   | Higher address
// |-----------------------------------|
// |                                   |
// | arguments passed on the stack     |
// |                                   |
// |-----------------------------------| <- sp
// |                                   | Lower address
< ASCII >
//
//
// After the prologue has run, the frame has the following general structure.
// Technically the last frame area (VLAs) doesn't get created until in the
// main function body, after the prologue is run. However, it's depicted here
// for completeness.
//
< ASCII >
// |                                   | Higher address
// |-----------------------------------|
// |                                   |
// | arguments passed on the stack     |
// |                                   |
// |-----------------------------------| <- (sp at function entry)
// |                                   |
// | varargs from registers            |
// |                                   |
// |-----------------------------------|
// |                                   |
// | prev_lr                           |
// | prev_fp                           |
// | (a.k.a. "frame record")           |
// |                                   |
// |- - - - - - - - - - - - - - - - - -| <- fp (r7 or r11)
// |                                   |
// | callee-saved gpr registers        |
// |                                   |
// |-----------------------------------|
// |                                   |
// | callee-saved fp/simd regs         |
// |                                   |
// |-----------------------------------|
// |.empty.space.to.make.part.below....|
// |.aligned.in.case.it.needs.more.than| (size of this area is unknown at
// |.the.standard.8-byte.alignment.....|  compile time; if present)
// |-----------------------------------|
// |                                   |
// | local variables of fixed size     |
// | including spill slots             |
// |-----------------------------------| <- base pointer (not defined by ABI,
// |.variable-sized.local.variables....|       LLVM chooses r6)
// |.(VLAs)............................| (size of this area is unknown at
// |...................................|  compile time)
// |-----------------------------------| <- sp
// |                                   | Lower address
< ASCII >
//
//
// To access the data in a frame, at-compile time, a constant offset must be
// computable from one of the pointers (fp, bp, sp) to access it. The size
// of the areas with a dotted background cannot be computed at compile-time
// if they are present, making it required to have all three of fp, bp and
// sp to be set up to be able to access all contents in the frame areas,
// assuming all of the frame areas are non-empty.
//
// For most functions, some of the frame areas are empty. For those functions,
// it may not be necessary to set up fp or bp:
// * A base pointer is definitely needed when there are both VLAs and local
//   variables with more-than-default alignment requirements.
// * A frame pointer is definitely needed when there are local variables with
//   more-than-default alignment requirements.
//
// In some cases when a base pointer is not strictly needed, it is generated
// anyway when offsets from the frame pointer to access local variables become
// so large that the offset can't be encoded in the immediate fields of loads
// or stores.
//
// The frame pointer might be chosen to be r7 or r11, depending on the target
// architecture and operating system. See ARMSubtarget::getFramePointerReg for
// details.
//
// Outgoing function arguments must be at the bottom of the stack frame when
// calling another function. If we do not have variable-sized stack objects, we
// can allocate a "reserved call frame" area at the bottom of the local
// variable area, large enough for all outgoing calls. If we do have VLAs, then
// the stack pointer must be decremented and incremented around each call to
// make space for the arguments below the VLAs.
//
//===----------------------------------------------------------------------===//
```
## Visual type:
- #memory-layout


== ./llvm/llvm_78.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/BPF/BPFInstrInfo.td#L125-L132

```c
// For arithmetic and jump instructions the 8-bit 'code'
// field is divided into three parts:
//
< ASCII >
//  +----------------+--------+--------------------+
//  |   4 bits       |  1 bit |   3 bits           |
//  | operation code | source | instruction class  |
//  +----------------+--------+--------------------+
//  (MSB)                                      (LSB)
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_79.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/BPF/BPFInstrInfo.td#L141-L147

```c
//For load and store instructions the 8-bit 'code' field is divided as:
//
< ASCII >
//  +--------+--------+-------------------+
//  | 3 bits | 2 bits |   3 bits          |
//  |  mode  |  size  | instruction class |
//  +--------+--------+-------------------+
//  (MSB)                             (LSB)
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_8.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libcxx/test/std/ranges/range.adaptors/range.elements/end.pass.cpp#L24-L30

```c
< ASCII >
// | simple | common |      v.end()     | as_const(v)
// |        |        |                  |   .end()
// |--------|--------|------------------|---------------
// |   Y    |   Y    |  iterator<true>  | iterator<true>
// |   Y    |   N    |  sentinel<true>  | sentinel<true>
// |   N    |   Y    |  iterator<false> | iterator<true>
// |   N    |   N    |  sentinel<false> | sentinel<true>
< ASCII >
```
## Visual type:
- #table


== ./llvm/llvm_80.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/Hexagon/HexagonFrameLowering.cpp#L67-L132

```c
// Hexagon stack frame layout as defined by the ABI:
//
< ASCII >
//                                                       Incoming arguments
//                                                       passed via stack
//                                                                      |
//                                                                      |
//        SP during function's                 FP during function's     |
//    +-- runtime (top of stack)               runtime (bottom) --+     |
//    |                                                           |     |
// --++---------------------+------------------+-----------------++-+-------
//   |  parameter area for  |  variable-size   |   fixed-size    |LR|  arg
//   |   called functions   |  local objects   |  local objects  |FP|
// --+----------------------+------------------+-----------------+--+-------
//    <-    size known    -> <- size unknown -> <- size known  ->
//
// Low address                                                 High address
//
// <--- stack growth
< ASCII >
//
//
// - In any circumstances, the outgoing function arguments are always accessi-
//   ble using the SP, and the incoming arguments are accessible using the FP.
// - If the local objects are not aligned, they can always be accessed using
//   the FP.
// - If there are no variable-sized objects, the local objects can always be
//   accessed using the SP, regardless whether they are aligned or not. (The
//   alignment padding will be at the bottom of the stack (highest address),
//   and so the offset with respect to the SP will be known at the compile-
//   -time.)
//
// The only complication occurs if there are both, local aligned objects, and
// dynamically allocated (variable-sized) objects. The alignment pad will be
// placed between the FP and the local objects, thus preventing the use of the
// FP to access the local objects. At the same time, the variable-sized objects
// will be between the SP and the local objects, thus introducing an unknown
// distance from the SP to the locals.
//
// To avoid this problem, a new register is created that holds the aligned
// address of the bottom of the stack, referred in the sources as AP (aligned
// pointer). The AP will be equal to "FP-p", where "p" is the smallest pad
// that aligns AP to the required boundary (a maximum of the alignments of
// all stack objects, fixed- and variable-sized). All local objects[1] will
// then use AP as the base pointer.
// [1] The exception is with "fixed" stack objects. "Fixed" stack objects get
// their name from being allocated at fixed locations on the stack, relative
// to the FP. In the presence of dynamic allocation and local alignment, such
// objects can only be accessed through the FP.
//
< ASCII >
// Illustration of the AP:
//                                                                FP --+
//                                                                     |
// ---------------+---------------------+-----+-----------------------++-+--
//   Rest of the  | Local stack objects | Pad |  Fixed stack objects  |LR|
//   stack frame  | (aligned)           |     |  (CSR, spills, etc.)  |FP|
// ---------------+---------------------+-----+-----------------+-----+--+--
//                                      |<-- Multiple of the -->|
//                                           stack alignment    +-- AP
< ASCII >
//
// The AP is set up at the beginning of the function. Since it is not a dedi-
// cated (reserved) register, it needs to be kept live throughout the function
// to be available as the base register for local object accesses.
// Normally, an address of a stack objects is obtained by a pseudo-instruction
// PS_fi. To access local objects with the AP register present, a different
// pseudo-instruction needs to be used: PS_fia. The PS_fia takes one extra
// argument compared to PS_fi: the first input register is the AP register.
// This keeps the register live between its definition and its uses.
```
## Visual type:
- #memory-layout


== ./llvm/llvm_81.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/Hexagon/HexagonInstrInfo.cpp#L3616-L3695

```c
// The diagram below shows the steps involved in the conversion of a predicated
// store instruction to its .new predicated new-value form.
//
// Note: It doesn't include conditional new-value stores as they can't be
// converted to .new predicate.
//
< ASCII >
//               p.new NV store [ if(p0.new)memw(R0+#0)=R2.new ]
//                ^           ^
//               /             \ (not OK. it will cause new-value store to be
//              /               X conditional on p0.new while R2 producer is
//             /                 \ on p0)
//            /                   \.
//     p.new store                 p.old NV store
// [if(p0.new)memw(R0+#0)=R2]    [if(p0)memw(R0+#0)=R2.new]
//            ^                  ^
//             \                /
//              \              /
//               \            /
//                 p.old store
//             [if (p0)memw(R0+#0)=R2]
< ASCII >
//
// The following set of instructions further explains the scenario where
// conditional new-value store becomes invalid when promoted to .new predicate
// form.
//
// { 1) if (p0) r0 = add(r1, r2)
//   2) p0 = cmp.eq(r3, #0) }
//
//   3) if (p0) memb(r1+#0) = r0  --> this instruction can't be grouped with
// the first two instructions because in instr 1, r0 is conditional on old value
// of p0 but its use in instr 3 is conditional on p0 modified by instr 2 which
// is not valid for new-value stores.
// Predicated new value stores (i.e. if (p0) memw(..)=r0.new) are excluded
// from the "Conditional Store" list. Because a predicated new value store
// would NOT be promoted to a double dot new store. See diagram below:
// This function returns yes for those stores that are predicated but not
// yet promoted to predicate dot new instructions.
//
< ASCII >
//                          +---------------------+
//                    /-----| if (p0) memw(..)=r0 |---------\~
//                   ||     +---------------------+         ||
//          promote  ||       /\       /\                   ||  promote
//                   ||      /||\     /||\                  ||
//                  \||/    demote     ||                  \||/
//                   \/       ||       ||                   \/
//       +-------------------------+   ||   +-------------------------+
//       | if (p0.new) memw(..)=r0 |   ||   | if (p0) memw(..)=r0.new |
//       +-------------------------+   ||   +-------------------------+
//                        ||           ||         ||
//                        ||         demote      \||/
//                      promote        ||         \/ NOT possible
//                        ||           ||         /\~
//                       \||/          ||        /||\~
//                        \/           ||         ||
//                      +-----------------------------+
//                      | if (p0.new) memw(..)=r0.new |
//                      +-----------------------------+
//                           Double Dot New Store
< ASCII >
//
// Returns the most basic instruction for the .new predicated instructions and
// new-value stores.
// For example, all of the following instructions will be converted back to the
// same instruction:
// 1) if (p0.new) memw(R0+#0) = R1.new  --->
// 2) if (p0) memw(R0+#0)= R1.new      -------> if (p0) memw(R0+#0) = R1
// 3) if (p0.new) memw(R0+#0) = R1      --->
//
// To understand the translation of instruction 1 to its original form, consider
// a packet with 3 instructions.
// { p0 = cmp.eq(R0,R1)
//   if (p0.new) R2 = add(R3, R4)
//   R5 = add (R3, R1)
// }
// if (p0) memw(R5+#0) = R2 <--- trying to include it in the previous packet
//
// This instruction can be part of the previous packet only if both p0 and R2
// are promoted to .new values. This promotion happens in steps, first
// predicate register is promoted to .new and in the next iteration R2 is
// promoted. Therefore, in case of dependence check failure (due to R5) during
// next iteration, it should be converted back to its most basic form.
```
## Visual type:
- #custom


== ./llvm/llvm_82.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/Hexagon/HexagonISelDAGToDAGHVX.cpp#L58-L102

```c
// Here's a delta network for 8 inputs, only the switching routes are
// shown:
< ASCII >
//
//         Steps:
//         |- 1 ---------------|- 2 -----|- 3 -|
//
// Inp[0] ***                 ***       ***   *** Out[0]
//           \               /   \     /   \ /
//            \             /     \   /     X
//             \           /       \ /     / \
// Inp[1] ***   \         /   ***   X   ***   *** Out[1]
//           \   \       /   /   \ / \ /
//            \   \     /   /     X   X
//             \   \   /   /     / \ / \
// Inp[2] ***   \   \ /   /   ***   X   ***   *** Out[2]
//           \   \   X   /   /     / \     \ /
//            \   \ / \ /   /     /   \     X
//             \   X   X   /     /     \   / \
// Inp[3] ***   \ / \ / \ /   ***       ***   *** Out[3]
//           \   X   X   X   /
//            \ / \ / \ / \ /
//             X   X   X   X
//            / \ / \ / \ / \
//           /   X   X   X   \
// Inp[4] ***   / \ / \ / \   ***       ***   *** Out[4]
//             /   X   X   \     \     /   \ /
//            /   / \ / \   \     \   /     X
//           /   /   X   \   \     \ /     / \
// Inp[5] ***   /   / \   \   ***   X   ***   *** Out[5]
//             /   /   \   \     \ / \ /
//            /   /     \   \     X   X
//           /   /       \   \   / \ / \
// Inp[6] ***   /         \   ***   X   ***   *** Out[6]
//             /           \       / \     \ /
//            /             \     /   \     X
//           /               \   /     \   / \
// Inp[7] ***                 ***       ***   *** Out[7]
//
< ASCII >
//
// Reverse delta network is same as delta network, with the steps in
// the opposite order.
//
//
// Benes network is a forward delta network immediately followed by
// a reverse delta network.
```
## Visual type:
- #custom


== ./llvm/llvm_83.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/Hexagon/HexagonPatterns.td#L2582-L2608

```c
// Preferred addressing modes for various combinations of stored value
// and address computation.
// For stores where the address and value are both immediates, prefer
// store-immediate. The reason is that the constant-extender optimization
// can replace store-immediate with a store-register, but there is nothing
// to generate a store-immediate out of a store-register.
//
< ASCII >
//         C     R     F    F+C   R+C   R+R   R<<S+C   R<<S+R
// --+-------+-----+-----+------+-----+-----+--------+--------
// C |   imm | imm | imm |  imm | imm |  rr |     ur |     rr
// R |  abs* |  io |  io |   io |  io |  rr |     ur |     rr
< ASCII >
//
// (*) Absolute or GP-relative.
//
// Note that any expression can be matched by Reg. In particular, an immediate
// can always be placed in a register, so patterns checking for Imm should
// have a higher priority than the ones involving Reg that could also match.
// For example, *(p+4) could become r1=#4; memw(r0+r1<<#0) instead of the
// preferred memw(r0+#4). Similarly Reg+Imm or Reg+Reg should be tried before
// Reg alone.
//
< ASCII >
// The order in which the different combinations are tried:
//
//         C     F     R    F+C   R+C   R+R   R<<S+C   R<<S+R
// --+-------+-----+-----+------+-----+-----+--------+--------
// C |     1 |   6 |   - |    5 |   9 |   - |      - |      -
// R |     2 |   8 |  12 |    7 |  10 |  11 |      3 |      4
< ASCII >
```
## Visual type:
- #table


== ./llvm/llvm_84.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/Hexagon/HexagonScheduleV60.td#L10-L53

```c
// There are four SLOTS (four parallel pipelines) in Hexagon V60 machine.
// This file describes that machine information.
//
< ASCII >
//    |===========|==================================================|
//    | PIPELINE  |              Instruction Classes                 |
//    |===========|==================================================|
//    | SLOT0     |  LD       ST    ALU32     MEMOP     NV    SYSTEM |
//    |-----------|--------------------------------------------------|
//    | SLOT1     |  LD       ST    ALU32                            |
//    |-----------|--------------------------------------------------|
//    | SLOT2     |  XTYPE          ALU32     J         JR           |
//    |-----------|--------------------------------------------------|
//    | SLOT3     |  XTYPE          ALU32     J         CR           |
//    |===========|==================================================|
< ASCII >
//
//
// In addition to using the above SLOTS, there are also six vector pipelines
// in the CVI co-processor in the Hexagon V60 machine.
//
< ASCII >
//      |=========| |=========| |=========| |=========| |=========| |=========|
// SLOT | CVI_LD  | |CVI_MPY3 | |CVI_MPY2 | |CVI_SHIFT| |CVI_XLANE| | CVI_ST  |
// ==== |=========| |=========| |=========| |=========| |=========| |=========|
// S0-3 |         | | CVI_VA  | | CVI_VA  | | CVI_VA  | | CVI_VA  | |         |
// S2-3 |         | | CVI_VX  | | CVI_VX  | |         | |         | |         |
// S0-3 |         | |         | |         | |         | | CVI_VP  | |         |
// S0-3 |         | |         | |         | | CVI_VS  | |         | |         |
// S0-1 |(CVI_LD) | | CVI_LD  | | CVI_LD  | | CVI_LD  | | CVI_LD  | |         |
// S0-1 |(C*TMP_LD) |         | |         | |         | |         | |         |
// S01  |(C*_LDU) | |         | |         | |         | | C*_LDU  | |         |
// S0   |         | | CVI_ST  | | CVI_ST  | | CVI_ST  | | CVI_ST  | |(CVI_ST) |
// S0   |         | |         | |         | |         | |         | |(C*TMP_ST)
// S01  |         | |         | |         | |         | | VSTU    | |(C*_STU) |
//      |=========| |=========| |=========| |=========| |=========| |=========|
//                  |=====================| |=====================|
//                  | CVI_MPY2 & CVI_MPY3 | |CVI_XLANE & CVI_SHIFT|
//                  |=====================| |=====================|
// S0-3             | CVI_VA_DV           | | CVI_VA_DV           |
// S0-3             |                     | | CVI_VP_DV           |
// S2-3             | CVI_VX_DV           | |                     |
//                  |=====================| |=====================|
//      |=====================================================================|
// S0-3 | CVI_HIST   Histogram                                                |
// S0123| CVI_VA_EXT Extract                                                  |
//      |=====================================================================|
< ASCII >
```
## Visual type:
- #custom


== ./llvm/llvm_85.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/Lanai/LanaiInstrFormats.td#L168-L195

```c
//------------------------------------------------------------------------------
// Register Memory (RM)
//------------------------------------------------------------------------------
< ASCII >
// Encoding:
//          -----------------------------------------------------------------
//          |1.0.0.S| . . . . | . . . . |P.Q| . . . . . . . . . . . . . . . |
//          -----------------------------------------------------------------
//           opcode     Rd        Rs1                 constant (16)
//
// Action:
//        Rd <- Memory(ea)      (Load)    see below for the
//        Memory(ea) <- Rd      (Store)   definition of ea.
< ASCII >
//
// `S' determines whether the instruction is a Load (0) or a Store (1).
// Loads appear in Rd one cycle after this instruction executes.  If the
// following instruction reads Rd, that instruction will be delayed by 1
// clock cycle.
//
< ASCII >
//   PQ      operation
//   --      ------------------------------------------
//   00      ea = Rs1
//   01      ea = Rs1,             Rs1 <- Rs1 + constant
//   10      ea = Rs1 + constant
//   11      ea = Rs1 + constant,  Rs1 <- Rs1 + constant
< ASCII >
//
// The constant is sign-extended for this instruction.
//
// A Jump is accomplished by `Rd' being `pc', and it has *two* delay slots.
```
## Visual type:
- #memory-layout


== ./llvm/llvm_86.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/Lanai/LanaiInstrFormats.td#L284-L326

```c
//------------------------------------------------------------------------------
// Conditional Branch (BR)
//------------------------------------------------------------------------------
< ASCII >
// Encoding:
//           -----------------------------------------------------------------
//           |1.1.1.0|D.D.D| . . . . . . . . . . . . . . . . . . . . . . |0.I|
//           -----------------------------------------------------------------
//            opcode condition                   constant (23)
//
// Action:
//            if (condition) { `pc' <- 4*(zero-extended constant) }
< ASCII >
//
// The BR instruction is an absolute branch.
// The constant is scaled as shown by its position in the instruction word such
// that it specifies word-aligned addresses in the range [0,2^25-4]
//
// The `DDDI' field selects the condition that causes the branch to be taken.
// (the `I' (Invert sense) bit inverts the sense of the condition):
//
< ASCII >
//   DDDI  logical function                        [code, used for...]
//   ----  --------------------------------------  ------------------------
//   0000  1                                       [T, true]
//   0001  0                                       [F, false]
//   0010  C AND Z'                                [HI, high]
//   0011  C' OR Z                                 [LS, low or same]
//   0100  C'                                      [CC, carry cleared]
//   0101  C                                       [CS, carry set]
//   0110  Z'                                      [NE, not equal]
//   0111  Z                                       [EQ, equal]
//   1000  V'                                      [VC, oVerflow cleared]
//   1001  V                                       [VS, oVerflow set]
//   1010  N'                                      [PL, plus]
//   1011  N                                       [MI, minus]
//   1100  (N AND V) OR (N' AND V')                [GE, greater than or equal]
//   1101  (N AND V') OR (N' AND V)                [LT, less than]
//   1110  (N AND V AND Z') OR (N' AND V' AND Z')  [GT, greater than]
//   1111  (Z) OR (N AND V') OR (N' AND V)         [LE, less than or equal]
< ASCII >
//
// If the branch is not taken, the BR instruction is a no-op.  If the branch is
// taken, the processor starts executing instructions at the branch target
// address *after* the processor has executed one more instruction.  That is,
// the branch has one “branch delay slot”.  Be very careful if you find yourself
// wanting to put a branch in a branch delays slot!
```
## Visual type:
- #memory-layout


== ./llvm/llvm_87.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/Lanai/LanaiInstrFormats.td#L468-L497

```c
//------------------------------------------------------------------------------
// Special Part-Word Load/Store (SPLS)
//------------------------------------------------------------------------------
< ASCII >
// Encoding:
//        -----------------------------------------------------------------
//        |1.1.1.1| . . . . | . . . . |1.1.0.Y.S.E.P.Q| . . . . . . . . . |
//        -----------------------------------------------------------------
//         opcode     Rd        Rs1                       constant (10)
< ASCII >
//
// Action:
//        If `YS' = 11  (bYte     Store):
//             Memory(ea) <- (least significant byte of Rr)
//        If `YS' = 01  (halfword Store):
//             Memory(ea) <- (least significant half-word of Rr)
//        If `YS' = 10  (bYte     load):  Rr <- Memory(ea)
//        If `YS' = 00  (halfword load):  Rr <- Memory(ea)
//             [Note: here ea is determined as in the RM instruction. ]
//        If `SE' = 01 then the value is zEro extended
//             before being loaded into Rd.
//        If `SE' = 00 then the value is sign extended
//             before being loaded into Rd.
//
// `P' and `Q' are used to determine `ea' as in the RM instruction. The
// constant is sign extended.  The timing is the same as the RM and RRM
// instructions.  *Note RM:: and *Note RRM::.
//
// All part-word loads write the part-word into the least significant
// part of the destination register, with the higher-order bits zero- or
// sign-extended.  All part-word stores store the least significant
// part-word of the source register into the destination memory location.
```
## Visual type:
- #memory-layout


== ./llvm/llvm_88.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/Lanai/LanaiInstrFormats.td#L525-L538

```c
//------------------------------------------------------------------------------
// Special instructions (popc, leadz, trailz)
//------------------------------------------------------------------------------
< ASCII >
// Encoding:
//         -----------------------------------------------------------------
//         |1.1.0.1|    Rd   |   Rs1   |F.-| . . . . | . . | . . . . | OP  |
//         -----------------------------------------------------------------
//          opcode      Rd       Rs1
< ASCII >
// Action:
//         Rd <- Perform action encoded in OP on Rs1
//   OP is one of:
//      0b001 POPC   Population count;
//      0b010 LEADZ  Count number of leading zeros;
//      0b011 TRAILZ Count number of trailing zeros;
```
## Visual type:
- #memory-layout


== ./llvm/llvm_89.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrArithmetic.td#L59-L65

```c
/// Encoding for Normal forms
< ASCII >
/// ----------------------------------------------------
///  F  E  D  C | B  A  9 | 8  7  6 | 5  4  3 | 2  1  0
/// ----------------------------------------------------
///             |         |         | EFFECTIVE ADDRESS
///  x  x  x  x |   REG   | OP MODE |   MODE  |   REG
/// ----------------------------------------------------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_9.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/libcxx/test/std/ranges/range.adaptors/range.join.view/end.pass.cpp#L28-L44

```c
< ASCII >
// | ID | outer  | outer   | outer  | inner | inner   | inner  |     end()     |    end()     |
// |    | simple | forward | common | l_ref | forward | common |               |    const     |
// |----|--------|---------|--------|-------|---------|--------|---------------|--------------|
// | 1  |   Y    |   Y     |   Y    |   Y   |    Y    |   Y    |iterator<true> |iterator<true>|
// | 2  |   Y    |   Y     |   Y    |   Y   |    Y    |   N    |sentinel<true> |sentinel<true>|
// | 3  |   Y    |   Y     |   Y    |   Y   |    N    |   Y    |sentinel<true> |sentinel<true>|
// | 4  |   Y    |   Y     |   Y    |   N   |    Y    |   Y    |sentinel<true> |      -       |
// | 5  |   Y    |   Y     |   N    |   Y   |    Y    |   Y    |sentinel<true> |sentinel<true>|
// | 6  |   Y    |   N     |   Y    |   Y   |    Y    |   Y    |sentinel<true> |sentinel<true>|
// | 7  |   N    |   Y     |   Y    |   Y   |    Y    |   Y    |iterator<false>|iterator<true>|
// | 8  |   N    |   Y     |   Y    |   Y   |    Y    |   N    |sentinel<false>|sentinel<true>|
// | 9  |   N    |   Y     |   Y    |   Y   |    N    |   Y    |sentinel<false>|sentinel<true>|
// | 10 |   N    |   Y     |   Y    |   N   |    Y    |   Y    |sentinel<false>|      -       |
// | 11 |   N    |   Y     |   N    |   Y   |    Y    |   Y    |sentinel<false>|sentinel<true>|
// | 12 |   N    |   N     |   Y    |   Y   |    Y    |   Y    |sentinel<false>|sentinel<true>|
< ASCII >
//
//
```
## Visual type:
- #table


== ./llvm/llvm_90.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrArithmetic.td#L115-L127

```c
< ASCII >
/// Encoding for Immediate forms
/// ---------------------------------------------------
///  F  E  D  C  B  A  9  8 | 7  6 | 5  4  3 | 2  1  0
/// ---------------------------------------------------
///                         |      | EFFECTIVE ADDRESS
///  x  x  x  x  x  x  x  x | SIZE |   MODE  |   REG
/// ---------------------------------------------------
///     16-BIT WORD DATA    |     8-BIT BYTE DATA
/// ---------------------------------------------------
///                 32-BIT LONG DATA
/// ---------------------------------------------------
< ASCII >
/// NOTE It is used to store an immediate to memory, imm-to-reg are handled with
/// normal version
```
## Visual type:
- #memory-layout


== ./llvm/llvm_91.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrArithmetic.td#L524-L528

```c
< ASCII >
/// ---------------------------------------------------
///  F  E  D  C  B  A  9 | 8  7  6 | 5  4  3 | 2  1  0
/// ---------------------------------------------------
///  0  1  0  0  1  0  0 |  OPMODE | 0  0  0 |   REG
/// ---------------------------------------------------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_92.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrArithmetic.td#L563-L569

```c
/// Word operation:
< ASCII >
/// ----------------------------------------------------
///  F  E  D  C | B  A  9 | 8  7  6 | 5  4  3 | 2  1  0
/// ----------------------------------------------------
///             |         |         | EFFECTIVE ADDRESS
///  x  x  x  x |   REG   | OP MODE |   MODE  |   REG
/// ----------------------------------------------------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_93.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrArithmetic.td#L745-L750

```c
< ASCII >
/// ------------+------------+------+---------+---------
///  F  E  D  C | B  A  9  8 | 7  6 | 5  4  3 | 2  1  0
/// ------------+------------+------+-------------------
///             |            |      | EFFECTIVE ADDRESS
///  0  1  0  0 | x  x  x  x | SIZE |   MODE  |   REG
/// ------------+------------+------+---------+---------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_94.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrBits.td#L30-L34

```c
< ASCII >
/// ------------+---------+---------+---------+---------
///  F  E  D  C | B  A  9 | 8  7  6 | 5  4  3 | 2  1  0
/// ------------+---------+---------+---------+---------
///  0  0  0  0 |   REG   | 1  0  0 |   MODE  |   REG
/// ------------+---------+---------+---------+---------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_95.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrBits.td#L45-L51

```c
< ASCII >
/// -------------------------------+---------+---------
///  F  E  D  C  B  A  9  8 . 7  6 | 5  4  3 | 2  1  0
/// -------------------------------+---------+---------
///  0  0  0  0  1  0  0  0 . 0  0 |   MODE  |   REG
/// ------------------------+------+---------+---------
///  0  0  0  0  0  0  0  0 |        BIT NUMBER
/// ------------------------+--------------------------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_96.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrControl.td#L89-L93

```c
< ASCII >
/// --------------------------------+---------+---------
///  F  E  D  C | B  A  9  8 | 7  6 | 5  4  3 | 2  1  0
/// --------------------------------+---------+---------
///  0  1  0  1 | CONDITION  | 1  1 |   MODE  |   REG
/// ----------------------------------------------------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_97.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrControl.td#L126-L130

```c
< ASCII >
///------------------------------+---------+---------
/// F  E  D  C  B  A  9  8  7  6 | 5  4  3 | 2  1  0
///------------------------------+---------+---------
/// 0  1  0  0  1  1  1  0  1  1 |  MODE   |   REG
///------------------------------+---------+---------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_98.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrControl.td#L239-L243

```c
< ASCII >
///------------------------------+---------+---------
/// F  E  D  C  B  A  9  8  7  6 | 5  4  3 | 2  1  0
///------------------------------+---------+---------
/// 0  1  0  0  1  1  1  0  1  0 |  MODE   |   REG
///------------------------------+---------+---------
< ASCII >
```
## Visual type:
- #memory-layout


== ./llvm/llvm_99.md
# https://github.com/llvm/llvm-project/blob/95e49f5a74c9e79778a62cc58b15875613cf9e59/llvm/lib/Target/M68k/M68kInstrData.td#L37-L44

```c
< ASCII >
/// -----------------------------------------------------
///  F  E | D  C | B  A  9 | 8  7  6 | 5  4  3 | 2  1  0
/// -----------------------------------------------------
///       |      |    DESTINATION    |       SOURCE
///  0  0 | SIZE |   REG   |   MODE  |   MODE  |   REG
/// -----------------------------------------------------
< ASCII >
///
/// NOTE Move requires EA X version for direct register destination(0)
```
## Visual type:
- #memory-layout
