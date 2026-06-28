# Code Review Guidelines

## To all team members

- Sandbox time daily to address review requests from your peers
- Review first, code second: Clear your code review queue first thing everyday, before working on your own code. 

## To authors

- Agree on the approach before implementation. The pull request should not come as a surprise to code owners.
- Keep pull requests small
- Provide a summary description: What is the change and why was it necessary
- Clarify expectations. Negotiatate with reviewers for their availability and response time.
- If a review drags beyond a day, get on a call and work it out 1:1 with reviewers. Avoid multiple rounds of comments on the same thread.

## To reviewers

- Make actionable suggestions
- Prefix a comment with `nit:` if it is nitpicky or expresses a personal preference
- Review AI generated code as if the author wrote it. Expect them to be able to explain it
- Expect 'correct' code, even at low TRLs. Reject pull request if:
  - The design intent and scope is not clear
  - The interfaces and APIs are prone to misuse
  - The implementation is inconsistent with system architecture and long term vision
  - The implementation fails basic software design principles as set out in the [developer guide](./developer_guide.md) (eg: DRY, SOLID, inconsistent units)
- Do not be tempted to accept work described as 'throw-away' code. They should stay in short-lived branches or private repositories instead. Rationale: Once in the main branch, 
  - Others will eventually depend on it, making it more permanent than you think
  - AI codegen tools will use it as context to generate new _poor quality_ code, potentially increasing technical debt
  - New joiners will see it as demonstrative examples of how to contribute to the codebase, and the quality that is expected.

## Automated CI checks

- The CI build should fail if:
  - The code is not formatted as per style guidelines
  - The code generates linter warnings
  - The code generates compiler warnings
  - Tests fail
- Do not merge pull requests that fails CI. 

## References

- [Sandor Dargo, Keeping code reviews from dragging](./media/dargo_code_reviews.pdf)