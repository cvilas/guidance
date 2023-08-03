# Guide to commit messages

- Follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
  - Rationale: Consistent format; human- and machine-parsable to generate meaningful changelogs between releases
- Structure

```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

- `type` can be of `build`, `chore`, `docs`, `feat`, `fix`, `perf`, `refactor`, `revert`, `style`, `test` 

- Example (`!` signifies breaking change): 

```text
feat(api)!: send an email to the customer when a product is shipped

Body of the message explaining the change in 
multiple lines.

Reference: https://github.com/cvilas/guidance/issues/7
```
