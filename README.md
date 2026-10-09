# juliet-c-benchmark

A small, curated subset of the NIST SARD **Juliet C/C++ Test Suite v1.3** (C files only), used as a labelled detection benchmark for static analysis.

Juliet is a work of the United States Government and is in the public domain.

## Source

- Upstream mirror: https://github.com/arichardson/juliet-test-suite-c
- Upstream commit: `f88433e3443648a17671398797a04ea1f8e1a274`
- Original: NIST Software Assurance Reference Dataset (SARD), Juliet C/C++ 1.3

## Contents

- `testcasesupport/`: the Juliet support files (`std_testcase.h`, `std_testcase_io.h`, `io.c`, `std_thread.c`, `std_thread.h`), copied unchanged so the test case headers resolve.
- `testcases/CWE.../`: 72 test case files, copied unchanged, keeping the upstream directory layout (including the `sNN/` sub-folders for the large CWEs).

## Selection rule

1. Only the `_01` flow variant (the baseline variant: source and sink in the same function, no control-flow obfuscation).
2. Only `.c` files (no `.cpp`).
3. At most 6 files per CWE. Where a CWE has many `_01` files, the 6 were picked to cover different data sources (console, environment, file, socket, rand, fixed value) and different sinks (for example `memcpy`, loop copy, `strcpy`, `strncpy`, `strcat` for the overflow CWEs).
4. CWEs covered (6 files each): CWE78, CWE121, CWE122, CWE134, CWE190, CWE191, CWE369, CWE401, CWE415, CWE416, CWE457, CWE476.
5. CWE23 and CWE36 (path traversal) were requested but have **no** `.c` files in Juliet 1.3 (every test case in those two CWEs is `.cpp`), so they are not in this subset.

## Ground truth

Each file contains one `..._bad()` function (flaw sites marked with `/* FLAW: ... */` or `/* POTENTIAL FLAW: ... */`) and one or more `good*()` functions (fixes marked with `/* FIX: ... */`). A `bad()` sink is a true vulnerability; a `good*()` sink is a false-positive trap.
