# Guidance on AI code generation tools

- Do not 'vibe-code' end-to-end. Leave that for weekend throwaway projects, as the person who coined the term said. You should be able to fully explain how your code works
- Write system design and system behaviour documentation yourself. Typically these go in the module README. These describe not just the _what_ but also the _why_. No AI can guess out-of-context assumptions and constraints simply by looking at the code.

## Rationale

- Writing is thinking. Do not delegate thinking away
- Auto-generated (sloppy) code unfairly shifts responsbility of code quality from the developer to the reviewers/maintainers. Code quality = passes our coding standards, is functional, is readable
