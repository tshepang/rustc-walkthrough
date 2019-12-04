# rustc walkthrough

*NOTE: the compiler is in a transition from being pass-based (sequential) to being
query-based (on-demand).*

## steps

Assumes we are doing a normal build, which means taking Rust source
code and transforming it into binary format (i.e. something a CPU will understand)

1. process compiler flags, of which there's plenty

1. lexing source code (converts Unicode characters into tokens)

1. parsing (turns tokens into an AST)

1. macro expansion and code elimination (i.e. cfg processing)

1. lower to HIR (includes sugar removal)

   note: query-based compilation begins here

1. type inference and type checking

1. lower to MIR (a far more simple version of "rust" which is suitable
   for borrow checking)

1. borrow checking (what makes Rust you-neek)

   Examples of what happens at this stage:
   - bindings can't be used uninitialised
   - values can't be accessed once moved
   - values can't be moved while still borrowed
   - values can't be accessed while still mutably borrowed

1. some optimization happens, to reduce the input to the next stage,
   which helps with build times

1. convert to LLVM IR

   Generics are expanded here (monomorphization)

1. (optional) optimize

1. convert to machine code (binary)

1. (in case of executables) linking

## more info

- https://rust-lang.github.io/rustc-guide
