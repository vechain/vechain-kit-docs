---
description: >-
  v2 introduces significant improvements including the replacement of `Connex`
  with improved developer experience
---

# v2 Migration

### What's Changed in 2.0

#### Major Breaking Changes

* **Connex Removal**: `useConnex` is replaced with `useThor`
* **New Contract Interaction Patterns**: Introduction of `useCallClause` and `executeMultipleClausesCall`
* **Enhanced Transaction Building**: New `useBuildTransaction` hook with improved type safety
* **Improved Type Safety**: Better TypeScript support with stricter typing

{% hint style="info" %}
`useCallClause` is for reading data from smart contracts with automatic\
caching and refetching

`executeMultipleClausesCall` is to execute multiple contract calls in a single batch
{% endhint %}

### Migration Path

#### Preparation Steps

1. Create a git branch for migration
2. Identify all places where `useConnex` is used (This is the main breaking change and easiest to find)
3. Run `tsc` compiler to see all broken references
4. Fix the compiler errors and migrate incrementally. The new methods provide type-safe returns that will guide you
5. Verify functionality as you fix each error
6. Ensure you have adequate test coverage before starting

### Getting Help

* **Documentation**: Refer to individual migration guide sections
* **GitHub Issues**: [Report issues](https://github.com/vechain/vechain-kit/issues)

### Next Steps

1. Start with the [API Changes](api-changes.md) guide to update your core dependencies and imports
2. Review [Contract Patterns](contract-patterns.md) to understand new interaction methods
3. Apply [Best Practices](best-practices.md) for optimal performance
4. Consult [Troubleshooting](troubleshooting.md) if you encounter issues
