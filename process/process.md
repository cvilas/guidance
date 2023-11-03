# Process

- Follow [developers' code](developers_code.md) for writing software
- Settle on a [release cadence](release_cadence.md) that encourages continuous delivery
- Set up the build system to generate and deploy artifacts in one step. Rationale: Enables frictionless deployment for testing and release.
- Maintain a bug database. Minimally, a bug report shall contain: steps to reproduce bug, expected behaviour, observed behaviour, who is it assigned to, fix schedule
- Use these [profiling tools](profiling.md) to measure performance

## Additional guidelines specific to industrial robotics development

- Build core technologies from first principles
- Option to delegate non-core technologies to a ‘few’ third party libraries, but:
  - Only choose third-party components that provide mechanisms (atomic capabilities) and do not impose policies (architectural limitations, design constraints) (Avoid 'frameworks' like ROS)
- Use [Layered Databus Architecture](lda.md) for large hierarchical distributed systems.
- Prefer behaviour trees over FSMs for task orchestration and fault recovery
- Develop tools that improve development velocity early:
  - Simulation framework from the get go
  - Log recording and playback framework for post-mortem analysis
- Use systems engineering languages: C++ (latest standard), Rust, Python
- Build system
  - Always current, and easily updatable to latest versions of compilers, languages, tools
  - Based on CMake, because our eco-system is built around it (not bazel or other esoteric tools)
  - Integrated unit test framework (catch2, googletest)
  - Integrated linting
  - Integrated sanitizers
  - Make it easy to write unit tests
  - One step automated testing
  - One step to build and deploy
- No separate Field Operations Team. Forward deploy members of the development team instead. Rationale:
  - Eliminates a layer of separation between builders and users, and enables lossless direct feedback
  - Engineers must 'eat their own dog food'
  - Effective diagnosis and problem-fixing in the field requires deep knowledge of how the system is engineered
  