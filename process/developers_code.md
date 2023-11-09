# The Developers' Code

## Brief

> "Code is a way you treat your coworkers." - Michael Feathers.

1. Design on paper first
1. Follow coding guidelines for correctness, style, and organisation
1. Document the work for future self, peers, and for posterity

## Design on paper first

> Writing is Thinking

Without committing to and aligning with _long-term_ vision for the product, it is impossible to develop software that is coherent over the long term. So review product concept documents first.

Resist the urge to start implementing without a _written_ design document, even if elementary and informal. This should, at a minimum, include:

- Assumptions made in order to arrive at the design
- The inputs, outputs, states and the intended behavior
- Interface specification and interaction with other components
- Failure modes and recovery mechanisms

It's acceptable for specifications to evolve with implementation, as we uncover previously overlooked issues. Embrace iterative development driven by field testing and feedback.

## Coding guidelines

> "C makes it easy to shoot yourself in the foot; C++ makes it harder, but when you do it blows your whole leg off." - Bjarne Stroustrup

- In using language features, be correct and safe. Use [CppCoreGuidelines](https://github.com/isocpp/CppCoreGuidelines). (example [clang-tidy](.clang-tidy) configuration)
- In coding _style_, be consistent. Follow [ROS C++ Style Guide](https://wiki.ros.org/CppStyleGuide). (example [.clang-format](.clang-format) configuration )
- Aim to implement code that is [correct by construction](correct_by_construction.md).
- Employ [third party software](third_party.md) sparingly, and with great caution and skepticism
- Follow a consistent format for [commit messages](commit_messages.md)
- Organise the implementation into logical 'modules', each with its own unit tests, example programs, and documentation. See [modules](#modules)
- When work is completed, raise a pull request and assign peers for [code review](code_reviews.md)

Anything that goes in the `main` branch is 'production code' even at low TRLs. Repo maintainers should reject pull requests if

- The work is not properly [documented](#document-the-work)
- The work disregards basic software design and engineering principles (eg: [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself), [SOLID](https://en.wikipedia.org/wiki/SOLID))
- The design is inconsistent with long-term vision for the product
- The code is not formatted as per style guidelines
- The code is not compliant with language usage rules (i.e. generates linter warnings)
- The code generates compiler warnings

## Document the work

> 'Code _is_ documentation'. However it is only an _implementation_ document, not a _design_ document.

Deviation from the design intent, inadvertent or deliberate, is usually a defect. This is impossible to detect without
corresponding design documentation.

- Colocate design documents and datasheets within codebase to make the codebase self-sufficient.
- Prefer simple data formats (`txt`, `md`)
- Implemented a new algorithm? Cite the source, or place relevant publications or even a copy of handwritten notes in the docs folder. The point is to sufficiently document the problem and the approach taken to solve it without forcing readers to reverse engineer the code or the implementer's thought process
- Provide code snippets and example programs to demonstrate intended usage
- Unless it's trivially obvious, document the API and parameters (eg: valid ranges, if it cannot be enforced in code, for instance, with `asserts`). Follow the doxygen convention in doing so.

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