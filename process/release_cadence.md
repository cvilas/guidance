# Release cadence

![Release Cadence](../media/release_cadence.png)

Follow a 'rolling release' development model (also called 'continuous delivery) that aims to release software to production bi-weekly. Every release candidate (RC) goes through two system-level validation and verification (V&V) tests, V&V1 and V&V2, conducted a week apart, before it is released.

- On the last day of a two-week Sprint, a new RC branch is generated, typically from the head of `main` branch.
- V&V process begins the following Monday (start of next Sprint), with V&V-1 and V&V-2 spaced one week apart.
  - If V&V-1 passes, the software is released to production, and V&V-2 is skipped.
  - If V&V-1 fails, the team shall triage and aim to resolve issues before V&V-2. Changes are integrated into the RC branch following the [hotfix procedure](#hotfixing-a-release-branch).
  - If V&V-2 passes, the software is released to production. If V&V-2 fails, the release RC is abandoned and the release skipped for the current cycle.

Note that if V&V-2 is skipped, we shall not expedite the next release; the next V&V-1 shall be scheduled as usual at the start of next Sprint. This fixed cadence gives each release at least two weeks to accumulate hours in production, and balances the interests of both the Development team (who want to see their features in production asap) and the Operations team (who value stability in production environment).

## Creating a release branch

- Create release branch from `main` branch. Call it `release-<version>`, where `version` is the version identifier.
- Mark the release branch protected so that only maintainers can modify this branch
- Build and package up the artifacts
- Publish

## Hotfixing a release branch

This procedure is for publishing a hotfix (eg: bug fix) for a release candidate (RC) or software in production.

**Note**: Never implement hotfixes directly into release branches. A fix must always be implemented into `main` branch first. This is to ensure that the head of the `main` branch is always 'green'.

- Create a branch from `main` and implement the fix in that branch
- Follow the normal development workflow to raise MR for the fix and merge the MR into `main` branch as an atomic commit
- Cherry pick the fix from `main` into the release branch, and tag it `release-<version>-hotfix-<nn>`, where
  - `nn` is a numeric sequence number, eg: `01` for first hotfix, `02` for second and so on.
- Build and package up the artifacts
- Publish
