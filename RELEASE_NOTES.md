## Highlights

- Set a Flight's instance size in `blueprint.yml` with `instanceType`: `F4`, `F16`, or `F32`. [#109](https://github.com/motherduckdb/motherduck-blueprints/pull/109)
- The local Dive preview now matches production, which no longer runs Dives cross-origin isolated. [#109](https://github.com/motherduckdb/motherduck-blueprints/pull/109)
- The action and the Python backend use DuckDB 1.5.6. [#109](https://github.com/motherduckdb/motherduck-blueprints/pull/109)

## Upgrading from v0.7.3

v0.7.4 is a patch release. Generated repositories pin an exact version in `Makefile` and in every workflow reference, so nothing changes until you upgrade. Workflows that reference the floating `@v0` tag receive v0.7.4 automatically.

1. Run `make upgrade VERSION=0.7.4`. It updates `CLI_VERSION` and every Blueprints workflow and action pin to `v0.7.4`.
2. Replace `.dive-preview/vite.config.ts` with the [v0.7.4 copy](https://github.com/motherduckdb/motherduck-blueprints/blob/v0.7.4/.dive-preview/vite.config.ts). `make upgrade` updates version pins only and does not change preview files.
3. Run `make validate`, and `make preview-smoke <blueprint-name>` for any Dive.
4. Open a pull request, review the diff, and merge it.

Before setting `instanceType`, read the [Compatibility](#compatibility) notes on plan limits and local deploys.

## Features

- Flight `instanceType` sets the Flight's size: `F4` (0.5 vCPU, 4 GB), `F16` (2 vCPU, 16 GB), or `F32` (4 vCPU, 32 GB). [#109](https://github.com/motherduckdb/motherduck-blueprints/pull/109)
  - Your plan decides the allowed sizes. Business allows all three. Lite and Free Trial allow `F4` and `F16`. Free allows `F4`.
  - Without the field, a new Flight gets the plan default, and an existing Flight keeps its current size. Removing the field does not reset the size.
  - Target overrides work, for example a smaller size for `preview`.
  - `md-blueprints import` records the size when the export includes it, and warns about sizes that cannot be declared, such as a retired `F8`.

## Bug Fixes

- A Dive that relies on `SharedArrayBuffer` used to work in the local preview and then fail in production, because the preview still sent cross-origin isolation headers that production no longer sends. The preview no longer sends them, so it fails locally the same way. [#109](https://github.com/motherduckdb/motherduck-blueprints/pull/109)

## Compatibility

- Sending `instanceType` needs DuckDB 1.5.6 or newer. The Blueprints action already uses it. Locally, Blueprints prefers the native MotherDuck CLI, which still embeds DuckDB 1.5.5. With `instanceType` set, `plan` and `deploy` stop before any write and explain this. Install `md-blueprints[deploy]` and set `MD_BLUEPRINTS_SQL_BACKEND=duckdb` to deploy locally. [#109](https://github.com/motherduckdb/motherduck-blueprints/pull/109)
- Imports through the native CLI do not include the instance size. Add `instanceType` before deploying an imported Flight to another target, or that target's Flight gets the plan default. [#109](https://github.com/motherduckdb/motherduck-blueprints/pull/109)
- `motherduck dive watch` shows a blank preview in CLI `v1.5.5` builds, including the version Blueprints installs. Use `make preview NAME=<blueprint-name>` until a newer CLI is published. [#109](https://github.com/motherduckdb/motherduck-blueprints/pull/109)

## Maintenance

- DuckDB is now 1.5.6 for the action and the `deploy` extra. MotherDuck supports this client. [#109](https://github.com/motherduckdb/motherduck-blueprints/pull/109)
- The field reference, adoption guide, CLI notes, and agent guides describe instance sizes. The package and action pins move to 0.7.4. [#109](https://github.com/motherduckdb/motherduck-blueprints/pull/109)

**Full diff:** https://github.com/motherduckdb/motherduck-blueprints/compare/v0.7.3...v0.7.4
