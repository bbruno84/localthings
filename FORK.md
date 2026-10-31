# Maintenance fork

This fork keeps the original `localthings` integration domain. Install it in place of the upstream integration; keep the existing config entries and entities. It is not a second integration.

## Release 0.35.0.post1

Based on upstream v0.35.0 with two independent fixes:

- Classify nginx HTTP 400 certificate errors during legacy token acquisition. If the generated CA-signed identity is refused, retry the supplied original identity once. Preserve the generated identity when it works, and do not reuse a legacy CA identity as a modern DTLS client leaf.
- For ARTIK051 power writes only, accept CoAP 4.00 only after a fresh successful GET confirms the complete requested power value. An unreadable, refused or different readback remains an error. Do not retry the power write itself.

Existing upstream fixes are inherited, including options-array readback matching and diagnostic Observe support. Household boost scripts remain Home Assistant configuration and are not part of this fork.

## Branches

- `main`: original upstream history.
- `casa`: household release branch, based on a stable upstream release plus the independent fixes and fork packaging.
- `fix/legacy-samsung-identity` and `fix/samsung-power-confirmation`: independent branches for upstream review.

## Install and recover

Back up Home Assistant before changing versions. Add `bbruno84/localthings` as a custom Integration repository in HACS, and download the tagged release. Disable updates from the original repository while using this fork. Restart Home Assistant and verify all existing devices and entity IDs. Do not delete and re-add configured appliances.

To recover, restore the component files and config-entry storage from the pre-upgrade backup, or restore that Home Assistant backup. Keep the previous working release until the new release has been checked on hardware.

## Updating from upstream

1. Fetch upstream branches and tags: `git fetch upstream --tags`.
2. Merge the next stable upstream tag into `casa`, reviewing each conflict and dropping fixes that upstream has incorporated.
3. Run all checks in CONTRIBUTING.md. Python 3.13 is a supported test interpreter; the pinned HA 2026.9.4 test environment has a Recorder annotation/autospec issue under Python 3.14.2.
4. Bump the fork post-release version and create a tagged GitHub release from the verified commit.
5. Install that exact release and check device identity, availability, readings and controls before advancing again.

No credentials, household configuration or raw appliance diagnostics belong in this repository.
