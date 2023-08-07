# Guidance

Documents and resources to  guide me as an engineer

## People

- [Team management](people/teams.md)

## Product

- Solving specific problems require deep understanding of the problem domain. So build that first.
  - Make it use-case driven. Generalise later, as more use-cases appear.
  - Let [Technology Readiness Levels](product/trl.md) guide product development.
    - Define a spec by TRL3.
    - Always report current TRL when updating stakeholders on progress.
- Identify V1 development platform asap.
  - Hardware components that cannot be abstracted away easily.
  - Stable hardware platform is paramount for software development velocity.
- Design the system from the UX end backwards, instead of slapping on a UI at the end.
  - Hire UX to lead product development beyond technical proof of concept (TRL3).
- Always have a plan B for high risk R&D. Implement the easier/obvious solution first and deliver, while the harder/better solution matures over time.
- [Layered Databus Architecture](product/lda.md) is suitable for large distributed systems.

## Process

- Follow [developers' code](./process/developers_code.md) for writing software
- Settle on a [release cadence](process/release_cadence.md) that encourages continuous delivery
- Set up the build system to generate and deploy artifacts in one step. Rationale: Enables frictionless deployment for testing and release.
- Maintain a bug database. Minimally, a bug report shall contain the following fields:
  - Complete steps to reproduce the bug
  - Expected behaviour
  - Observed behaviour
  - Who is it assigned to
  - Whether it is fixed or not
- Use these [profiling tools](process/profiling.md) to measure performance

Additional tips specific to robotics product development

- Delegate non-core technologies to a ‘few’ third party libraries
  - utilities: absl, glog, gflags, google config
  - serdes: Protobuf
  - Task orchestration and fault recovery: Behaviour Trees
  - 3D visualisation: Unreal engine (talent pool) or Ogre (simplicity)
  - 2D gui: Imgui or Qt
  - Data transport: DDS or ZeroMQ within product. MQTT for external interfaces.
- Build core technologies from first principles, including all robotics
  - No ROS
- Develop tools that improve development velocity early:
  - Simulation framework from the get go
  - Logging framework that is conducive to post-mortem analysis
- Use systems engineering languages: C++ (latest standard), Rust, Python
  - Use C++ exceptions. Stopped being an issue in hard realtime systems since C++98
  - Don't allow macros
- Build system
  - Always current, and easily updatable to latest versions of compilers, languages, tools
  - Based on CMake, because our eco-system is built around it (not bazel or other esoteric tools)
  - Integrated unit test framework (catch2, googletest)
  - Integrated linting
  - Integrated sanitizers
  - Make it easy to write unit tests
  - One step automated testing
  - One step to build and deploy
- Interaction with field operations
  - Integrate Dev team into Ops, because Ops may have insufficient troubleshooting skills. Without this integration, Ops will keep interrupting Devs.
  - Allow Dev team to enter ‘flow’ state everyday. Ops should interact with Dev via specific non-interruptive modes of communication.
  