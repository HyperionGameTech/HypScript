# HypScript

HypScript was the in-house scripting language of the Hyperion Engine: a
dynamically-typed language compiled to bytecode and executed on a register-based
VM with garbage collection and deep engine-reflection integration (values were
engine `BoxedValue`s; arrays, strings, maps, names and math were exposed as
reflection-registered methods). Language features included closures, structs and
classes with inheritance, enums, exceptions, varargs, default/named arguments,
references, and hot reload.

**This repository is an archival snapshot. It is not maintained and will not
receive updates.**

HypScript was superseded by **Strata**, the Hyperion Engine's statically-typed,
LLVM-based (JIT + AOT) scripting language.

## Contents

- `Core/` — verbatim snapshot of the engine's core library (containers,
  reflection, memory, logging, utilities), which `Lang/` depends on.
- `Lang/` — verbatim snapshot of the HypScript implementation: lexer, parser,
  semantic analyzer, bytecode emitter, VM, and garbage collector.
- `LICENSE` — MIT, as in the engine.

This is **not** a standalone buildable project. The build system, generated
reflection sources, and all engine-side integration points were not extracted.

## Provenance

- Extracted verbatim from the Hyperion Engine repository.
- Snapshot commit: `b286162ed479ee3ec5803607dfa90763b569c571`
- Snapshot date: 2026-09-04

## What stayed in the engine

The runtime integration lived outside `Lang/` and remains in the engine
repository:

- `Source/Engine/Scripting/` — script object lifecycle and entity-scripting glue
- `Source/Engine/Scripting/Wrappers/` — reflection registrations for the
  array/string/map/name/math builtins
- `Tools/CodeGen` — `HypScriptModuleGenerator`, which generated `Lib.hyp` (the
  engine API bindings loaded at startup)
- `Source/Engine/Test/Script/` — functional tests and the Strata/HypScript
  benchmark harness

## Why it was retired

Measured with the engine's script-language benchmark (release build, x64,
best-of-5):

- ~12–18x slower than CPython 3.14 on compute workloads, up to ~35x on
  string/array workloads. The VM performed per-instruction type dispatch on
  type-erased values with no inline caches, specialization, or unboxed fast
  paths, and dispatched builtins through reflection member lookups.
- Strata (LLVM JIT) executed the same workloads ~100–800x faster.

The engine is migrating all scripting to Strata.
