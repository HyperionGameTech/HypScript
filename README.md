# HypScript

HypScript was the in-house scripting language of the Hyperion Engine: a
dynamically-typed language compiled to bytecode and executed on a register-based
VM with garbage collection and deep engine-reflection integration (values were
engine `BoxedValue`s; arrays, strings, maps, names and math were exposed as
reflection-registered methods).

Language features included closures, structs and
classes with inheritance, enums, exceptions, varargs, default/named arguments,
references, and hot reload (with the engine).

**This repository is an archival snapshot. It is not maintained and will not
receive updates.**

HypScript was superseded by **Strata**, the Hyperion Engine's statically-typed,
LLVM-based (JIT + AOT) scripting language.

## Contents

- `Core/` : verbatim snapshot of the engine's core library (containers,
  reflection, memory, logging, utilities), which `Lang/` depends on.
- `Lang/` : verbatim snapshot of the HypScript implementation: lexer, parser,
  semantic analyzer, bytecode emitter, VM, and garbage collector.
- `LICENSE` : MIT as it was in the engine project.

This is **not** a standalone buildable project. The build system, generated
reflection sources, and all engine-side integration points were not extracted.

- Extracted verbatim from the Hyperion Engine repository.
- Snapshot date: 2026-09-04

## Why was it retired?

The engine is migrating all scripting to Strata, a new language that's JIT compiled in editor and allows for AOT compilation via LLVM.

Measured with the engine's script-language benchmark (release build, x64,
best-of-5), Strata (LLVM JIT) executed ~100–800x faster.

