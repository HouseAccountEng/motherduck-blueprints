## Highlights

- The local Dive preview supports `useDiveState`, so Dives that keep filters, sort order, or selected views in shared state now preview locally. [#104](https://github.com/motherduckdb/motherduck-blueprints/pull/104)
- Release commits pass the package version check even when the tag is pushed before CI reaches that step. [#96](https://github.com/motherduckdb/motherduck-blueprints/pull/96)

## Upgrading from v0.7.1

v0.7.2 is a patch release with no manifest or configuration changes. Generated repositories pin an exact version in `Makefile` and in every workflow reference, so nothing changes until you upgrade. Workflows that reference the floating `@v0` tag receive v0.7.2 automatically.

1. Run `make upgrade VERSION=0.7.2`. It updates `CLI_VERSION` and every Blueprints workflow and action pin to `v0.7.2`.
2. To use `useDiveState` in local previews, replace `.dive-preview/src/md-sdk.tsx` with the [v0.7.2 copy](https://github.com/motherduckdb/motherduck-blueprints/blob/v0.7.2/.dive-preview/src/md-sdk.tsx). `make upgrade` updates version pins only and does not change preview files. No new npm packages are needed.
3. Run `make validate`, and `make preview-smoke <blueprint-name>` for any Dive.
4. Open a pull request, review the diff, and merge it.

## Features

- Local Dive preview: `useDiveState(key, initialValue)` returns a `useState`-style value and setter. State is kept in a `diveState` URL parameter, so it survives reloads and copied preview links. Setting a key to `undefined` restores its initial value, and call sites with the same key share state. Like the Dive runtime, the preview rejects values that are not JSON-serializable and state over 64 KB. [#104](https://github.com/motherduckdb/motherduck-blueprints/pull/104)

## Bug Fixes

- CI accepts the package version on the commit its release tag points to, so a release commit no longer fails the version check when the tag is pushed first. [#96](https://github.com/motherduckdb/motherduck-blueprints/pull/96)

## Documentation

- The setup guide explains how an unapproved production run holds later deploys, and the action guide explains how preview cleanup treats Dependabot branches. [#97](https://github.com/motherduckdb/motherduck-blueprints/pull/97)

**Full diff:** https://github.com/motherduckdb/motherduck-blueprints/compare/v0.7.1...v0.7.2
