# Process

- Follow the [developer's guide](developer_guide.md) for writing software
- Build core technologies from first principles
- Avoid third-party libraries unless [absolutely necessary](third_party.md).
- Use [Layered Databus Architecture](lda.md) for large hierarchical distributed systems.
- Prefer Behaviour Trees over FSMs for task orchestration and fault recovery. Rationale: debuggability, extendabilty
- Develop tools that improve development velocity early:
  - Simulation environment from the beginning
  - Data recording and playback for post-mortem analysis
- Use systems engineering languages: C++, Rust, Python
- Maintain a bug database. Minimally, a bug report shall contain:
  - Steps to reproduce bug
  - Expected behaviour
  - Observed behaviour
- Use these [profiling tools](profiling.md) to measure performance
- Build system
  - Always current, and easily updatable to latest versions of compilers, languages, tools. Rationale: 
    For greenfield projects, there is no reason to consider anything but the cutting edge.
  - Based on CMake, because our eco-system is built around it (not bazel or other esoteric tools)
  - Integrated unit test framework (catch2, googletest)
  - Integrated linting
  - Integrated sanitizers
  - One step process to build, test and deploy.
- Release into production often on a fixed [release cadence](release_cadence.md)
- No separate Field Operations Team. Forward deploy members of the development team instead. Rationale:
  - Eliminates a layer of separation between builders and users, and enables lossless direct feedback
  - Engineers must 'eat their own dog food'
  - Effective diagnosis and problem-fixing in the field requires deep knowledge of how the system is engineered
- Minimise distance (layers of management, reporting line) between field operators and decision makers. 
  Rationale: Motivates timely action on business risks or personnel safety.
