# Guide to commit messages

Follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) for consistent format that is human- and machine-parsable to generate meaningful changelogs between releases

## Structure

```text
<type>(scope): <description>

[optional body: Provides additional implementation detail if necessary]

[optional footer(s): Reference to issue tracker]
```

type: `build`, `chore`, `docs`, `feat`, `fix`, `perf`, `refactor`, `revert`, `style`, `test` 

## Example 

```text
feat(api)!: Sends an email to the customer when a product is shipped

Adds integration with email client and customer contact information backend to autogenerate and dispatch email with shipping information. 

Reference: https://github.com/<organisation>/<project>/issues/7
```

(`!` signifies breaking change)

