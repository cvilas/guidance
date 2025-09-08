# The Developers' Code

## Brief

> "Code is a way you treat your coworkers." - Michael Feathers.

1. Follow branch and merge model for integrating code changes
1. Follow coding guidelines for correctness, style, and organisation
1. Document the work for future self, peers, and for posterity

## Follow branch and merge model for integrating code changes

- Create a new branch from `main` to work on your feature or fix
- Follow coding guidelines (see subsequent section)
- Document your work (see subsequence section)
- When work is completed,
  - Tidy up commit history with `git rebase`. Follow guidelines for [commit messages](commit_messages.md).
  - Test your work in simulation and real hardware as appropriate.
  - Raise a merge request and have it [peer reviewed](code_reviews.md) before merging.
- Follow a continuous delivery model to release feature into production. See [release cadence](release_cadence.md).

It's acceptable for specifications to evolve with implementation, as we uncover previously overlooked issues. Embrace iterative development driven by field testing and feedback.

## Coding guidelines

> "C makes it easy to shoot yourself in the foot; C++ makes it harder, but when you do it blows your whole leg off." - Bjarne Stroustrup

- In using language features, be correct and safe. Use [CppCoreGuidelines](https://github.com/isocpp/CppCoreGuidelines). (example [clang-tidy](.clang-tidy) configuration)
- In coding _style_, be consistent. Follow [ROS C++ Style Guide](https://wiki.ros.org/CppStyleGuide). (example [.clang-format](.clang-format) configuration )
- Aim to implement code that is [correct by construction](correct_by_construction.md).
- Employ [third party software](third_party.md) sparingly, and with great caution and skepticism
- Organise the implementation into logical 'modules', each with its own unit tests, example programs, and documentation. See [modules](#modules)
- Use AI coding assistants to improve productivity but do so [thoughtfully](ai.md) 

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

## Document the work

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
- Provide code snippets and example programs to demonstrate intended usage
- Unless it's trivially obvious, document the API and parameters (eg: valid ranges, if it cannot be enforced in code, for instance, with `asserts`). Follow the doxygen convention in doing so.
