# Product

- Solving specific problems require deep understanding of the problem domain. So build that first. Document it.
- Make _it_ use-case driven. Generalise later, as more use-cases appear.
- Let [Technology Readiness Levels](trl.md) guide product development.
  - Do [requirements analysis](../process/requirements.md) to define a spec by TRL3.
  - Report current TRL when updating stakeholders on progress.
  - Follow a *no renders* policy. No special effects. No edits that hide issues.
- With hardware development, front-load effort on design.
  - Hardware iterations are slow and expensive. Aim to get to the right design with fewest iterations.
  - Hardware cannot be abstracted away easily. Make it representative of the final thing as soon as possible
  - Stable hardware platform is paramount for software development velocity. Identify V1.0 hardware development platform ASAP.
- With software development, 'bias to action'
  - Rationale: Unlike hardware, software iterations are fast and cheap
  - However, aim to get the fundamental architecture right asap. Reworking the fundamentals gets expensive over time 
- Focus on the UX at every level
  - Component level: Clear APIs that are [correct by construction](../process/correct_by_construction.md)
  - Product level: Approach system design from the UX-end, instead of slapping on a UI at the end of system development.
  - Hire UX experts to lead product development beyond technical proof of concept (TRL3).
  - Train all engineers on basic principles of usability [1]
  - Simplify workflows and user interactions required to achieve a goal with our product
- Simplify the tech stack
  - Do not conflate simple with easy/familiar. [2] [3]
  - Simplicity != effortless. It takes time and effort to simplify. Make that investment.
- Always have a plan B for high risk R&D.
- Most progress happens at system integration and delivery time, _after_ we think we are done. Build up the energy in the dev team for this phase of work

## Notes

- [1] Read [The design of everyday things](https://www.amazon.co.uk/Design-Everyday-Things-MIT-Press/dp/0262525674)
- [2] [Simple made easy](https://youtu.be/SxdOUGdseq4?si=h-VFjYRghysS92bu)
- [3] In the world of traditional watchmaking, any functionality that does not contribute to 
      displaying time is called a ['complication'](https://en.wikipedia.org/wiki/Complication_(horology)). 
      An inspired bit of naming.