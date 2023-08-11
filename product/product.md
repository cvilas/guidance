# Product

- Solving specific problems require deep understanding of the problem domain. So build that first.
  - Make it use-case driven. Generalise later, as more use-cases appear.
  - Let [Technology Readiness Levels](trl.md) guide product development.
    - Define a spec by TRL3.
    - Always report current TRL when updating stakeholders on progress.
- Identify V1 hardware development platform asap.
  - Hardware components cannot be abstracted away easily. Aim the get to the right design with fewest iterations.
  - Stable hardware platform is paramount for software development velocity.
- Design the system from the UX end backwards, instead of slapping on a UI at the end.
  - Hire UX to lead product development beyond technical proof of concept (TRL3).
- Always have a plan B for high risk R&D. Implement the easier/obvious solution first and deliver, while the harder/better solution matures over time.
- [Layered Databus Architecture](lda.md) is suitable for large distributed systems.
