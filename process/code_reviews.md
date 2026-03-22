# Code Review Guidelines

## To all team members

- Set aside time for unplanned review requests from your peers every sprint.
- 'Throw-away' code shall stay in short-lived branches and shall not be merged into main branch.
  If it is in the main branch,
  - Others will eventually depend on it, making it more permanent than you think
  - AI codegen tools may use it as context to generate new code, potentially increasing technical debt
  - New joiners may see it as demonstrative examples of how to contribute to the codebase, and
    the quality that is expected.

## To authors

- In the pull request, define
  - What is the change and why was it necessary
  - Who reviews what, if the diff is non-trivial
- Write example programs to clarify usage where necessary
- Write unit tests to cover potential violations of assumptions and contracts in the future
- Document the work as per coding guidelines.

## To reviewers

- Make actionable suggestions
- Prefix a comment with `nit:` if it is nitpicky or expresses a personal preference
- AI generated code shall be reviewed as if the author wrote it and are able to explain it
- Expect 'correct' code, even at low TRLs. Reject pull request if:
  - The design intent and scope is not clear
  - The interfaces and APIs are prone to misuse
  - The implementation is inconsistent with system architecture and long term vision
  - The implementation fails basic software design principles as set out in the [developer guide](./developer_guide.md) (eg: DRY, SOLID, inconsistent units)

## Automated CI checks

- The CI build should fail if:
  - The code is not formatted as per style guidelines
  - The code generates linter warnings
  - The code generates compiler warnings
  - Tests fail
- Do not merge pull requests that fails CI. 

## Tips for speeding up code reviews

- Keep pull requests small
- Clarify expectations. Negotiatate with reviewers for their availability and response time.
- If the change is sophisticated enough, the problem it solves and the general approach should have
  been agreed upon prior to implementation. The pull request should not come as a surprise to code owners.
