# Developer Guidelines

## Brief

> "Code is a way you treat your coworkers." - Michael Feathers.

1. Maintain the codebase as a [collection of modules](#modules)
2. Follow [coding guidelines](#coding-guidelines) for correctness, style, and organisation
3. [Document](#documentation) the work for future self, peers, and for posterity
4. Follow [branch and merge model](#branch-and-merge-model) for integrating code changes upstream

### Modules

Organise the code repository as a collection of _modules_. A module is a cohesive group of

- libraries
- example programs
- unit tests
- executables
- systemd services
- documentation
- configuration files
- scripts

A suggested directory structure is as follows:

- src (containing implementation files)
- include (headers)
- tests (unit tests)
- examples (demonstrative examples)
- docs (datasheets, design docs)
- scripts (tools, etc)

## Coding guidelines

> "C makes it easy to shoot yourself in the foot; C++ makes it harder, but when you do it blows your whole leg off." - Bjarne Stroustrup

- In using language features, be correct and safe. Follow [CppCoreGuidelines](https://github.com/isocpp/CppCoreGuidelines), with automated linting ([example config](.clang-tidy))
- In coding _style_, be consistent. Follow [ROS C++ Style Guide](https://wiki.ros.org/CppStyleGuide) with automated formatting. ([example config](.clang-format))
- Aim to implement code that is [correct by construction](correct_by_construction.md) and therefore, hard to misuse
  - Prefer `const`, `consteval`, `constexpr`; use `enum class` over bare enums or `bool` flags
  - Use strong/named types to clarify semantics; mark single-argument constructors `explicit`
  - Use `static_assert` for compile-time checks; prefer `switch` with no `default` over `if/else` chains
  - Use RAII everywhere (Constructor Acquires, Destructor Releases)
  - Use contracts to protect invariants in code (`contract_assert`, `pre(condition)`, `post(r: condition)`)
- Follow basic software design principles of [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) and [SOLID](https://en.wikipedia.org/wiki/SOLID)
- Use SI units in public interfaces
- Prefer functional programming and free-standing interfaces to minimise coupling and dependencies within and between modules. See [modules](#modules)
- In writing applications, follow UNIX philosophy of programs that (a) do one thing and do it well and (b) work well together by composition
- Employ [third party software](third_party.md) sparingly, and with great caution and skepticism
- Add example programs, where appropriate, to clarify usage of newly introduced features
- Add unit tests, where appropriate, to test newly added functionality
- Use AI coding assistants to improve productivity but do so [thoughtfully](ai.md) 

## Documentation

> 'Code _is_ documentation'. However it is only an _implementation_ document, not a _design_ document.

Deviation from the design intent, inadvertent or deliberate, is usually a defect. This is impossible to detect without corresponding design documentation.

- Design documentation includes
  - Assumptions made in order to arrive at the design
  - The inputs, outputs, states and the intended behavior
  - Interface specification and interaction with other components
  - Failure modes and recovery mechanisms
- Colocate design documents and datasheets within codebase to make the codebase self-sufficient.
- Prefer simple data formats (`txt`, `md`)
- Implemented a new algorithm? Cite the source, or place relevant publications or even a copy of handwritten notes in the docs folder. Do not force readers to reverse engineer the code or the implementer's thought process
- Unless it's trivially obvious, document the API and parameters (eg: valid ranges, if it cannot be enforced in code, for instance, with `asserts`). Follow the doxygen convention in doing so.

# Branch and merge model

- Create a new branch from `main` to work on your feature or fix
- Maintain a linear and atomic commit history. Tidy up commits and rebase them over the `main` branch with `git rebase`. Follow formatting [guidelines](commit_messages.md) for commit messages.
- Test your work in simulation and real hardware as appropriate.
- Raise a merge request and have it [reviewed](code_reviews.md) before merging.
- Follow a continuous delivery model to release feature into production. See [release cadence](release_cadence.md).

It's acceptable for specifications to evolve with implementation, as we uncover previously overlooked issues. Embrace iterative development driven by field testing and feedback.
