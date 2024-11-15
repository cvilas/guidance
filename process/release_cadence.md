# Release cadence

![Release Cadence](../media/release_cadence.png)

## Principles

- The `main` branch is always 'green' (i.e. functional)
- Releases are created from `main` branch and delivered to production bi-weekly.
  - This cadence is fixed. Release what is ready. Do not delay release for a feature. Rationale: Production environment requires predictable schedules
- Every release candidate (RC) gets two validation and verification (V&V) tests, before release.

## Procedure

- On the last day of a two-week Sprint, generate a new RC branch from the head of `main` branch.
- Start V&V-1 the following Monday (start of next Sprint). V&V-1 and V&V-2 spaced one week apart.
  - If V&V-1 passes, release software to production. Skip V&V-2.
  - If V&V-1 fails, attempt to triage and resolve issues before V&V-2. Integrate any changes into the RC branch following the [hotfix procedure](#hotfixing-a-release-branch).
  - If V&V-2 passes, release software to production. 
  - If V&V-2 fails, abandon the RC and skip the release for the current cycle.

Note that if V&V-2 is skipped, we shall not expedite the next release; the next V&V-1 shall be scheduled as usual at the start of next Sprint. This fixed cadence gives each release at least two weeks to accumulate hours in production, and balances the interests of both the development team (who want to see their features in production asap) and the operations team (who value stability in production environment).

### Creating a release branch

- Create release branch from `main` branch. Call it `release-<version>`, where `version` is the version identifier.
- Mark the release branch protected so that only maintainers can modify this branch
- Build and package up the artifacts
- Publish

### Hotfixing a release branch

This procedure is for publishing a hotfix (eg: bug fix) for a release candidate (RC) or software in production.

**Note**: Never implement hotfixes directly into release branches. A fix must always be implemented into `main` branch first. This is to ensure that the head of the `main` branch is always 'green'.

- Create a branch from `main` and implement the fix in that branch
- Follow the normal development workflow to raise MR for the fix and merge the MR into `main` branch as an atomic commit
- Cherry pick the fix from `main` into the release branch, and tag it `release-<version>-hotfix-<nn>`, where
  - `nn` is a numeric sequence number, eg: `01` for first hotfix, `02` for second and so on.
- Build and package up the artifacts
- Publish
