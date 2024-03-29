# How to integrate third-party software

## General guidelines

- Avoid third-party libraries where possible.
  - Certainly avoid 'frameworks'.
  - Certainly avoid third party libraries as a way to outsource core competencies.
- Integrate a third-party library into the codebase _only when_ it does not force design compromises.

### Rationale

1. Design core components from first principles, building only what we need, following our design guidelines. Re-invent the wheel. This approach results in a system that is simpler, easier to understand, extend and maintain.
1. We _own_ any third-party code we integrate, i.e., we become responsible for its bug fixes and maintenance. Customer operations cannot stop and wait for original authors of a library to fix the software upstream. Better to write and fix your own bugs than deal with others'.

## HowTo

If the benefits of using third-party libraries outweigh the aforementioned drawbacks, then:

- Ensure licensing terms allow commercial usage. See [Approved Licenses](#approved-licenses)
- Prefer libraries from vendors who offer paid support. Rationale: To seek help if we are stuck.
- Hide third-party headers and API behind our own wrappers. See [Hide third-party API](#hide-third-party-api). Rationale: Avoids incompatibility with system-installed versions of those libraries.
- Build third-party libraries from source as part of our build. See [Integrating third-party dependencies](#integrating-third-party-dependencies). Rationale: Makes the codebase self-contained, and allows third-party libraries to be customised for our needs (eg: enabling only features we want, custom patches, etc).

## Approved licenses

Approved licenses            | Denied licenses (Do not use)
---------------------------- | -----------------------------
BSD                          | GPL
MIT                          | LGPL with static linking
Apache                       |
Mozilla Public License       |
LGPL with dynamic linking    |

If a license is not in the list of approved licenses, talk to the software vendor to agree on terms of use.

## Hide third-party API

- Avoid `#include <third_party_lib.h>` in public-facing headers. Instead, put them in `.cpp` or private implementation headers (following _pimpl_ technique).
- Avoid using third-party data structures and data types in our public-facing API.

## Integrating third-party dependencies

Use:

- `FetchContent` to integrate third-party sources directly into our codebase (as if our own), and build using the same configuration as the rest of our own codebase (eg: compiler settings, etc)
- `ExternalProject_Add` to integrate third-party sources with custom build configuration isolated from rest of our codebase.
