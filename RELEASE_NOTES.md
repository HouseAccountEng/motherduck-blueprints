## Highlights

- Pull request previews deploy only the packages a change affects. Editing one Dive no longer redeploys every other Dive that reads the same producer. [#106](https://github.com/motherduckdb/motherduck-blueprints/pull/106)
- The preview comment is short. It names the changed packages and any packages added for dependencies, lists the preview links, and folds the full resource table into a collapsed section. [#106](https://github.com/motherduckdb/motherduck-blueprints/pull/106)

## Upgrading from v0.7.2

v0.7.3 is a patch release with no manifest or configuration changes. Generated repositories pin an exact version in `Makefile` and in every workflow reference, so nothing changes until you upgrade. Workflows that reference the floating `@v0` tag receive v0.7.3 automatically.

1. Run `make upgrade VERSION=0.7.3`. It updates `CLI_VERSION` and every Blueprints workflow and action pin to `v0.7.3`.
2. Run `make validate`.
3. Open a pull request, review the diff, and merge it. That pull request's own preview comment shows the new layout.

Check the preview behavior described under [Compatibility](#compatibility) if your reviewers rely on seeing every Dive in a preview.

## Bug Fixes

- Previews no longer deploy unrelated packages. Before, a preview deployed every package connected to a change, so a one-line edit to one Dive also deployed its producer and every other Dive reading that producer. Now a preview deploys the changed packages, their downstream consumers, and the producers those packages read. [#106](https://github.com/motherduckdb/motherduck-blueprints/pull/106)
- The preview pull request comment no longer repeats the resource list. Before, it showed the plan table, the preview links, and a verification table, so the same resources appeared up to three times. Now it shows: [#106](https://github.com/motherduckdb/motherduck-blueprints/pull/106)
  - a **Selected** line with the changed packages, and **Added by the dependency graph** for packages deployed only as producers
  - the preview links for each package
  - the verified resource table in a collapsed section
- When a long comment is truncated inside the collapsed section, the section is closed so the truncation notice stays visible. [#106](https://github.com/motherduckdb/motherduck-blueprints/pull/106)

## Compatibility

- Previews include fewer packages. A Dive that reads the same producer as a changed Dive, but is not changed itself, is no longer deployed to the preview. To preview it, change it in the same pull request or run **Deploy Blueprints** manually for `preview` with that package selected. Production and staging selection is unchanged, and preview cleanup still removes every resource on the branch. [#106](https://github.com/motherduckdb/motherduck-blueprints/pull/106)
- The preview plan table moved from the pull request comment to the workflow run summary. `plan` and `verify` output, and the deployment verification summary, now start with the **Selected** line when packages are selected. Update any tooling that parses that output. [#106](https://github.com/motherduckdb/motherduck-blueprints/pull/106)

## Maintenance

- Bump the package and action pins to 0.7.3. [#106](https://github.com/motherduckdb/motherduck-blueprints/pull/106)

**Full diff:** https://github.com/motherduckdb/motherduck-blueprints/compare/v0.7.2...v0.7.3
