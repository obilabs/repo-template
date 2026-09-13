# Releasing

## Feature maturity

Every feature flag carries one maturity level, defined once in the code registry:

| Maturity | Development profile | Release profile |
|---|---|---|
| `stable` | on | on |
| `preview` | on | off by default; an admin can enable it, shown with a "Preview" badge |
| `experimental` | on | absent: not in navigation, routes or settings |

The profile comes from configuration (for example `FEATURE_PROFILE=release|development`).
Published images and production compose files default to `release`; development and test
stacks use `development`.

A test fails the build if, in the release profile, any experimental feature resolves on, any
navigation item lacks a registered flag, or two navigation entries lead to the same page.

## Promoting a feature to stable

A feature is stable only when all of these hold:

- It has tests covering its main path and its failure states.
- No "coming soon", placeholder or `alert()` text is reachable.
- It is documented for the person who will use it.
- It meets the UI bar: shared components, honest empty and error states, no dead links.

## Cutting a version

1. All CI green on `main`, including the hygiene check.
2. Review which flags are `preview`; promote only what meets the bar above.
3. Tag `vMAJOR.MINOR.PATCH` with the noreply identity; release notes say what works, what is
   preview, and what is untested.
