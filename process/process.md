# Process

- Follow the [developer's guide](developer_guide.md) for writing software
- Build core technologies from first principles
- Option to delegate non-core technologies to a ‘few’ third party libraries, but:
  - Only choose third-party components that provide mechanisms (atomic capabilities), not policies (how they are used). Rationale: Policies impose architectural limitations and design constraints
  - Avoid 'frameworks': ROS, Qt
- Use [Layered Databus Architecture](lda.md) for large hierarchical distributed systems.
- Prefer behaviour trees over FSMs for task orchestration and fault recovery. Rationale: debuggability, extendabilty
- Develop tools that improve development velocity early:
  - Simulation framework from the get go
  - Log recording and playback framework for post-mortem analysis
- Use systems engineering languages: C++ (latest standard), Rust, Python
- Maintain a bug database. Minimally, a bug report shall contain:
  - Steps to reproduce bug
  - Expected behaviour
  - Observed behaviour
- Use these [profiling tools](profiling.md) to measure performance
- Build system
  - Always current, and easily updatable to latest versions of compilers, languages, tools
  - Based on CMake, because our eco-system is built around it (not bazel or other esoteric tools)
  - Integrated unit test framework (catch2, googletest)
  - Integrated linting
  - Integrated sanitizers
  - Make it easy to write unit tests
  - One step automated testing
  - One step to build and deploy. Rationale: Enables frictionless deployment for testing and release.
- Settle on a [release cadence](release_cadence.md) that encourages continuous delivery
- No separate Field Operations Team. Forward deploy members of the development team instead. Rationale:
  - Eliminates a layer of separation between builders and users, and enables lossless direct feedback
  - Engineers must 'eat their own dog food'
  - Effective diagnosis and problem-fixing in the field requires deep knowledge of how the system is engineered
- Minimise distance (layers of management, reporting line) between field operators and decision makers. Rationale: Motivates timely action on issues of high risk to business or personnel safety [1].

## Notes

[1] Whether it's the O-rings on the Challenger or foam-strike on Columbia, the engineers on the ground knew. The message lost it's urgency on the way up the chain of command.
