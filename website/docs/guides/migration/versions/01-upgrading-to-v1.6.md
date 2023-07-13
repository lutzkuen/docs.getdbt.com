---
title: "Upgrading to v1.6 (prerelease)"
description: New features and changes in dbt Core v1.6
---

:::warning Prerelease

dbt Core v1.6 is available as a release candidate. Final release planned for July 27.

:::

## Resources

- [Changelog](https://github.com/dbt-labs/dbt-core/blob/1.6.latest/CHANGELOG.md)
- [CLI Installation guide](/docs/core/installation)
- [Cloud upgrade guide](/docs/dbt-versions/upgrade-core-in-cloud)
- [Release schedule](https://github.com/dbt-labs/dbt-core/issues/7481)

## What to know before upgrading

dbt Labs is committed to providing backward compatibility for all versions 1.x, with the exception of any changes explicitly mentioned below. If you encounter an error upon upgrading, please let us know by [opening an issue](https://github.com/dbt-labs/dbt-core/issues/new).

### Behavior changes

- dbt Core v1.6 does not support Python 3.7, which reached End Of Life on June 23. Support Python versions are 3.8, 3.9, 3.10, and 3.11.
- As part of the Semantic layer re-launch (in beta), the spec for `metrics` has changed significantly. Migration guide coming soon: https://github.com/dbt-labs/docs.getdbt.com/pull/3705
- Manifest schema version is now v10, reflecting [TODO] changes

## New and changed documentation
- [Revamped dbt Semantic Layer pages](/docs/use-dbt-semantic-layer/dbt-sl)
- [dbt Semantic Layer API](/docs/use-dbt-semantic-layer/sl-api-overview)
- [dbt Semantic Layer manifest](/docs/use-dbt-semantic-layer/sl-manifest)
- [Legacy dbt Semantic Layer migration guide](guides/migration/sl-migration)
### Materialized views

**Materialized view** support (for model and project configs) has been added for three data warehouses:
- [Bigquery](/reference/resource-configs/bigquery-configs#materialized-view)
- [Postgres](/reference/resource-configs/postgres-configs#materialized-view)
- [Redshift](/reference/resource-configs/redshift-configs#materialized-view)
- Snowflake dynamic tables ([docs TK](https://github.com/dbt-labs/docs.getdbt.com/issues/3494))
- Databricks (implementation & docs TK)

### New commands for mature deployment

[`dbt retry`](/reference/commands/retry) executes the previously run command from the point of failure. Rebuild just the nodes that errored or skipped in a previous run/build/test, rather than starting over from scratch.

`dbt clone` ([docs TK](https://github.com/dbt-labs/docs.getdbt.com/issues/3607)) leverages each data platform's functionality for creating lightweight copies of dbt models from one environment into another. Useful when quickly spinning up a new development environment, or promoting specific models from a staging environment into production.

### Namespacing

[Model names](/faqs/Models/unique-model-names) can be duplicated across different namespaces (packages/projects), so long as they are unique within each package/project. We strongly encourage using [two-argument `ref`](/reference/dbt-jinja-functions/ref#two-argument-variant) when referencing a model from a different package/project.

More consistency and flexibility around packages! Resources defined in a package will respect variable and global macro definitions within the scope of that package.
- `vars` defined in a package's `dbt_project.yml` are now available in the resolution order when compiling nodes in that package, though CLI `--vars` and the root project's `vars` will still take precedence. See ["Variable Precedence"](/docs/build/project-variables#variable-precedence) for details.
- `generate_x_name` macros (defining custom rules for database, schema, alias naming) follow the same pattern as other "global" macros for package-scoped overrides. See [macro dispatch](/reference/dbt-jinja-functions/dispatch) for an overview of the patterns that are possible.

### Quick hits

- [`state:unmodified` and `state:old`](/reference/node-selection/methods#the-state-method)
