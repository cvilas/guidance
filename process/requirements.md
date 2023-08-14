# How to write good requirements

![Requirements](../media/dilbert_user_requirements.png "requirements")

> Requirement: A capability or condition that must (or should) be satisfied - SysML standard.

Requirements analysis makes a design problem tractable, by defining bounds for what a system must do, under what conditions (assumptions and contraints), and how well.

To initiate requirements gathering for a component, start with answers to these questions:

1. _Why_ is the system required? (i.e. What is the Problem we want to solve and why is it worth solving?)
1. _What_ is the system intended to do? (i.e. What must be achieved? What is the benefit if the Problem is solved.)
1. What is the _boundary/scope_? What elements are within the system and what are outside?
1. _Who_ interacts with the system?
1. _When_ do interactions take place? (dynamic behaviour)
1. _How_ do interactions take place? (order of events)

## Viewpoint-Oriented Requirements Modelling

This method identifies stakeholders and their vested interests, and the system is modelled from their viewpoint. Stakeholders can be classified into Users, Beneficiaries, and Constrainers.

#### Users

- Directly **interacts** with the system. They may be human, other systems or the physical environment.
- Are concerned with **functionality** (what it does and when).
- Defines **interfaces** (i.e. information passed, feedback mechanisms, its format, timing, protocol, units)

> Users define **functional** goals

#### Beneficiaries

- **Benefits** from having the system
- Have a **problem that needs resolving** (i.e. needs your system to  do something they can't on their own)
- Typically **pays** for the system
- Concerned with **QoS** - performance, throughput, reliability, maintainability.
- Usually **not interested in details** of the functionality (how the system does what it does)

> Beneficiaries define **QoS** goals

#### Constrainers

- Places limitations, 'negative requirements'.  Defines what the system **must not do**.
- Enforces **compliance** with regulations for safety, quality, codes of conduct
- Defines **limitations**: Of technology, resources, and skills available within the development team.
- Sets down **assumptions** about the system, the operating environment (eg: temperature), and those interacting.

> Constrainers establish design **constraints and limitations**, and ultimately makes the engineering problem tractable

### Modelling process

This approach enables analysis from outside-in. Top level requirements (eg objectives from the vision spec.) get defined first. This defines the higher level system components, which lends itself to further decomposition into a hierarchy of smaller (simpler) components.

Any given level/sub-system defines _what_ must be achieved in order to satisfy the levels above.

The higher levels typically clarify _why_ a sub-system must do what it does, and the lower levels describes _how_ it might be achieved.

The above stakeholder analysis will lead a set of statements that we call _primary requirements_.
The above is seldom sufficiently detailed to implement a solution. Enhance further with information from the problem domain to develop _derived requirements_. These are not explicitly stated in the stakeholder requirements yet the system is required to satisfy them.

> Requirements are complete when all stakeholder needs are addressed.

### Format

The following comes from Rolls Royce EARS paper (see References). Read it. It's only 6 pages.

Generate a table where each row lists an individual requirement in the following format (items in [brackets] are optional). This will later form the basis for the Traceability Matrix where columns describe where they come from and how they are verified.

```text
[Trigger] [and Precondition] the Actor shall Action [the Object]
```

The 'special words' are

- **Shall** - an absolute requirement that must be met
- **Should** - a desirable requirement that would be beneficial if met
- **Will** - A statement of fact, truth.
Do not use 'Must'.

The granularity of definition of requirements (i.e. which requirements you want to set in stone and when) depends on the Technology Readiness of the components that interact with the sub-system. For instance, an external system with higher level of maturity will demand a more detailed interface specification for your sub-system if your sub-system interacts with it. Similarly, avoid pinning decisions down in relation to other less-well-defined subsystems, allowing requirements to evolve as different elements of the system mature over time.

Examples
These examples are from Intel's report on applying EARS in their corporate environment. See ears_intel.pdf 

EARS Pattern | Use | Example
---|---|---|
Ubiquitous | (_precondition_) (_trigger_) The (_system name_) shall (_response_) | _(System_) shall have a matte finish.
Event-driven | WHEN (_trigger_) the (_system name_) shall (_response_).| When _button_ is depressed, the system ringer shall silence.
Unwanted behaviour | IF (_unwanted trigger_), THEN the (_system name_) shall (_response_)| If A/C power is lost, then the backup battery shall power the system with no interruption in service.
State-driven |WHILE (_in specific state_), the (_system name_) shall (_response_) | While system is in S1 state, the volume button shall remain functional.
Optional | WHERE (_feature is included_), the (_system name_) shall (_response_). | Where dual-band radios are available, 3G shall receive priority.
Complex | (combine multiple patterns) | If A/C power is lost while video is playing on-screen, the screen shall dim.

## Validation and Verification

Validation is about suitability of requirement specifications, whereas verification is about compliance with those specifications.

- Validation: Get the requirements table reviewed by peers and by stakeholders. Punch holes. Iterate until all agree on the requirements.
- Verification: Requirements must yield test conditions which are later used to define tests. Tests verify whether a requirement has been met. It naturally follows that
  - Test conditions must be measurable.
  - Requirements without measurable goals, pre- and post- conditions cannot be verified.
- Update the verification column in the traceability matrix with test conditions

### Validation Checklist

Use the following checklist to validate your set of requirements. 

- Each requirement is expressed as a single statement in the format suggested above.
- Each requirement is implementation neutral (It does not force a particular solution by specifying implementation details or sequence of operations. A requirement must state what is needed, not how to provide it.)
- Clarity checks. Ensure that language
  - Uses active voice
  - Is grammatically correct
  - Does not use negation. (State a requirement in terms of a positive action.)
  - Does not use double negatives.
  - Is concise and simple. Don't make the reader work hard to decipher your intent.
  - Is unambiguous. (Do not use terms such as: etc, and/or, as appropriate, adequate, user-friendly, flexible, easy, sufficient.)
- Consistency checks:
  - All terminology and abbreviations are defined.
  - SI units are used for all physical quantities.
  - No requirements that conflict each other
- Completeness checks
  - All assumptions are explicitly stated.
  - All external and internal interfaces between sub-systems are defined
  - For qualitative or performance measures, tolerances are specified (e.g. '... the robot shall dock successfully in less than 3 attempts')
  - For quantifiable measures, exact values or a numerical ranges are specified
- Is necessary and sufficient
  - Ensure that requirements are together sufficient to achieve a successful project outcome
  - Ensure that the requirement is technically feasible and testable. Think about how the requirement will be verified as met. Define the criteria for acceptance
  - Is the requirement a duplicate or unnecessary?
  - Does the requirement over-constrain the design without reason?
  - Is the requirement eventually traceable to a higher-level requirement or vision specification.

### Verification

The system can be verified to meet requirements via one or more of the following methods:

- By inspection/examination of design drawings, circuit diagrams, software and their construction (weak)
- By direct measurement of behaviour with practical tests (preferred)

## References

- Feabhas SysML Training notes
- Rolls Royce's Easy Approach to Requirements Syntax (EARS)
