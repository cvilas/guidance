# Guidance on AI code generation tools

LLM-based coding agents are a superpower and a force multiplier. Use it. Watch the following talk to:
- Understand what LLMs are
- What they are good at and not good at (as of 2025)
- How to architect your software to make the most of them

[Learning To Stop Writing C++ Code (and Why You Won’t Miss It) - Daisy Hollman - ACCU 2025](https://youtu.be/mpGx-_uLPDM)

## Hints

- Use LLM agents as your peer programmer or life coach. Ask questions with the intent to seek knowledge
- Do not 'vibe-code' end-to-end. Leave that for weekend throwaway projects, as the person who coined the term said. You should be able to fully explain how your code works
- Software designed to be fit for humans are also fit for LLMs
  - Good abstractions, low coupling/high cohesion, DRY, SOLID, comments and docs are all quite important still, as they also help provide the AI with the context it needs to generate code
  - Provide an initial system design in the form of description of interfaces and behaviours. Typically these go in the module README. These describe not just the _what_ but also the _why_. No AI can guess out-of-context assumptions and constraints simply by looking at the code.

## Finally

- Writing is thinking. Do not delegate thinking away
- Don't let auto-generated code unfairly shift responsbility of code quality from you as the developer to someone else (reviewers, maintainers, users)
