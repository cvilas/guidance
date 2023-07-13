# Reviewing work samples for Software Engineering roles

Published: [Medium 2021-03-23](https://vilaschitrakaran.medium.com/guidelines-for-reviewing-work-samples-for-software-engineering-roles-eba9bdef0084)

In the past, I have [talked about](https://vilaschitrakaran.medium.com/colleagues-wanted-4b293c6f1062) what I look for in my peers and what kind of team I would like to build. As a team leader, I interview people quite a lot, and we do our best to let our Applicants shine through. For software roles, I have resisted coding assignments as a means to filter candidates, and focus on code reviews instead. I elaborate on the ‘why’ in [another post](https://vilaschitrakaran.medium.com/why-we-dont-insist-on-homework-assignments-for-recruiting-d408a5769330). This post sets out a proposal for how we should review work samples from applicants.

A good engineer is more than a programmer. What we want are work samples that best reflect the Applicant as an engineer, and is representative of how they work. Therefore, the emphasis is on review of their existing work; explicit coding exercises are unlikely to reveal all the attributes we want to see. If the Applicant is unable to provide us with work samples, a short coding assignment should be considered, with a well defined rubric for assessment that the Applicant is made aware of in advance.

In the design of tangible objects, there are [principles](https://www.amazon.co.uk/Design-Everyday-Things-Revised-Expanded/dp/0465050654/) you can follow that lead to useful, aesthetic, desirable and [pleasurable products](https://www.vitsoe.com/gb/about/good-design) [book](https://www.amazon.co.uk/Dieter-Rams-Principles-Good-Design/dp/3791383663/). In the software world, we have the [SOLID](https://en.wikipedia.org/wiki/SOLID) principles. Design, whether in the tangible world or in software, is still somewhat subjective. We are generally looking for the following attributes in a good code sample:

## Simplicity of design from a user’s perspective

- Is the public API intuitive and minimal.
- Does it nudge the user in the right direction and stop inadvertent misuse (concept of [affordances](https://medium.com/@sachinrekhi/don-normans-principles-of-interaction-design-51025a2c0f33))
- Are the API and usage patterns documented?
- Does it build the first time, if you follow the instructions?

## Simplicity of implementation

- Does it achieve what it needs to in the fewest lines of code
- Does it exploit existing algorithms in the C++ standard library
- Does it use modern C++ features (they usually simplify code)
- If the implementation is esoteric or complex, is there a reference to a publication or document explaining it?
- Is it implemented in a way others can pick up, understand, use and extend?
- Does it follow well-established principles of design (i.e. SOLID)

## Correctness

- Does it do what it’s supposed to do, or what it says it does?
- Are the contracts on inputs and outputs enforced?
- Algorithms and mathematical models typically work under certain assumptions about the operating envelope of the system. Are the assumptions validated? (Are they documented, at least)
- Are violations caught at compile time (preferred), load time (ok) or runtime (not always ideal)?
- Are there unit tests?

## Error handling

- Does it have any?
- If so, is it an after-thought or an integral part of the design?

(Note: We build software for robots. Hence, the emphasis on C++ as the programming language.)

Designing simplicity into a product requires expert knowledge gained over years of experience. Hence, the emphasis on simplicity — it’s a good indicator of maturity of the Applicant. Software development is also a social and collaborative activity, and quite a few attributes listed above emphasise this aspect of it (eg: documentation, user-friendliness, developer-friendliness and maintainability).

The review process: Consider collaboratively reviewing, critiquing, debugging and extending the code-sample during the interview, preferably on the Applicant’s laptop/PC with development tools of their choice. Rationale: This is representative of how we work and much better than a whiteboard algorithm implementation exercise.

Ultimately, we should be mindful that code review is only one component of the interview process. Even for a software role, it isn’t necessarily the most important thing. Personal attitudes, culture fit and aptitude to learn still count a fair amount.
