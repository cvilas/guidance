# Code Review Guidelines

### To all team members

- Set aside time in your sprint to account for unplanned review requests from your colleagues.

### To MR authors

- Please give reviewers a heads-up on what the MR is about (if not in the MR description already).
- Empathise: You are asking for unplanned time from your colleague, who may be busy on their own tasks. Work with them personally to get your MR through in a timely manner.

### To reviwers

- Make your availability known when your review is requested. Clarify expected response time.
- In reviewing an MR
  - Make actionable suggestions
  - Focus on API clarity and user experience. Less than perfect internal details could be improved in future MRs
- Expect production-quality code, even at low TRLs. Reject MR if:
  - The work is not properly documented as per coding guidelines.
  - The work disregards basic software design and engineering principles (eg: [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself), [SOLID](https://en.wikipedia.org/wiki/SOLID))
  - The implementation is inconsistent with system architecture znd long term vision for the product
  - The code is not formatted as per style guidelines
  - The code is not compliant with language usage rules (i.e. generates linter warnings)
  - The code generates compiler warnings

For additional guidance, follow [Google Code Review guidelines](https://google.github.io/eng-practices/review/).
