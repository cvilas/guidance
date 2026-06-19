# How to modernise a legacy codebase

```
DRAFT
```

## Why

In the best case, a codebase before and after modernisation will appear to provide no additional functional benefits to those who paid for it. In the worst case, for sufficiently complex codebases, the risk of breaking what used to work is rather high. 

Acceptance of change is inversely proportional to risk. Few and careful changes to business critical code

Examples of changes that are good:

- Deletes lines-of-code (net) without losing functionality
- Simplifies
- Robustifies
- Modernises
- Increases static analysis rigour
- Introduces sanitizers in CI
- Increases test coverage
- Low risk 
- Easily reversible
- Localised 

## How

### Capture baseline

- Benchmark current state
  - Build time
  - Code coverage
  - Performance metrics (CPU/GPU/memory usage, throughput, whatever is appropriate for subset of codebase where it matters)
- Write missing unit tests and improve coverage where necessary
- Modify CI to capture these metrics. This is your baseline

### Make improvement

After each step, compare against baseline for regressions. Fix if any.

- Upgrade toolchain (compiler, language version). 
- Improve consistency in code formatting. Enforce with `.clang-format`. 
- Improve consistency in usage of language features. Enforce with `.clang-tidy`.
- Modernise to improve code quality (RAII, etc)

To be continued...