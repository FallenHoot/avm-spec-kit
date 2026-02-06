---
description: "Check AVM module dependencies for updates and breaking changes"
tools: ["fetch", "read_file", "grep_search", "github-pull-request_doSearch", "todos"]
---

# AVM Dependency Check

You are checking the user's AVM module for dependency updates and potential breaking changes.

## Step 1: Find Module References

Scan the user's Bicep files for AVM module references:

```bicep
// Pattern to find:
module xyz 'br/public:avm/res/...' = { }
module abc 'br/public:avm/ptn/...' = { }
```

Use `grep_search` to find:
- `br/public:avm/res/`
- `br/public:avm/ptn/`
- `br/public:avm/utl/`

## Step 2: Parse Dependencies

For each module reference found, extract:
- Module path (e.g., `avm/res/storage/storage-account`)
- Current version (e.g., `0.9.0`)

Create a dependency list:
```
## Dependencies Found

| Module | Current Version | File | Line |
|--------|-----------------|------|------|
| avm/res/storage/storage-account | 0.9.0 | main.bicep | 45 |
| avm/res/network/private-endpoint | 0.4.0 | main.bicep | 78 |
```

## Step 3: Check Latest Versions

For each dependency, fetch the latest version:

1. Check the module's CHANGELOG.md in bicep-registry-modules:
   `https://github.com/Azure/bicep-registry-modules/blob/main/avm/res/{module}/CHANGELOG.md`

2. Or check the Bicep registry (if accessible)

Report:
```
## Version Comparison

| Module | Current | Latest | Gap | Action |
|--------|---------|--------|-----|--------|
| storage/storage-account | 0.9.0 | 0.11.0 | 2 minor | ⚠️ Consider update |
| network/private-endpoint | 0.4.0 | 0.4.0 | - | ✅ Current |
| key-vault/vault | 0.5.0 | 0.7.0 | 2 minor | ⚠️ Consider update |
```

## Step 4: Analyze Breaking Changes

For each outdated dependency, check for breaking changes:

1. Fetch CHANGELOG.md
2. Look for entries between current and latest version
3. Flag any "BREAKING CHANGE" or major version bumps

Report:
```
## Breaking Change Analysis

### avm/res/storage/storage-account (0.9.0 → 0.11.0)

#### v0.11.0
- ⚠️ BREAKING: `storageAccountSku` renamed to `sku`
- Added: `minimumTlsVersion` parameter

#### v0.10.0
- No breaking changes
- Added: `allowBlobPublicAccess` parameter

### Upgrade Risk Assessment
| Module | Risk | Reason |
|--------|------|--------|
| storage-account | 🔴 High | Breaking parameter rename |
| private-endpoint | 🟢 Low | Already current |
```

## Step 5: Check for Deprecations

Look for deprecation notices:

```
## Deprecation Warnings

| Module | Status | Notice |
|--------|--------|--------|
| [module] | ⚠️ Deprecated | Replaced by [new module] |
| [module] | ✅ Active | No deprecation planned |
```

## Step 6: Generate Update Plan

If updates are recommended:

```
## Recommended Update Plan

### Priority 1: Security Updates
- [ ] Update X to vY.Z (security fix)

### Priority 2: Breaking Changes (Plan Carefully)
- [ ] Update storage-account 0.9.0 → 0.11.0
  - Change: `storageAccountSku` → `sku`
  - Files affected: main.bicep (line 45)
  - Test: Run defaults test after change

### Priority 3: Feature Updates (Optional)
- [ ] Update key-vault 0.5.0 → 0.7.0
  - New features available: [list]
  - No breaking changes

### Update Commands
```bicep
// Before
module storage 'br/public:avm/res/storage/storage-account:0.9.0' = { }

// After
module storage 'br/public:avm/res/storage/storage-account:0.11.0' = { }
```
```

## Step 7: Track with Todos

Use `#todos` if updates are needed:
- [ ] Review breaking changes for [module]
- [ ] Update [module] from vX to vY
- [ ] Test after update
- [ ] Update CHANGELOG.md
