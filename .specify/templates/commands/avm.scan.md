---
description: "Scan AVM repos for issues, PRs, and changes relevant to your module"
tools: ["fetch", "github-pull-request_doSearch", "github-pull-request_formSearchQuery", "github-pull-request_issue_fetch", "todos"]
---

# AVM Repository Scan

You are scanning AVM-related repositories for issues, PRs, and changes that could affect the user's module.

## Input Required

Ask the user for:
1. **Module path** (e.g., `avm/ptn/finops-toolkit/finops-hub`)
2. **Keywords** related to their module (e.g., "FinOps", "Cost Management", "Data Explorer")
3. **Time range** (default: last 30 days)
4. **For PTN modules**: Ask for the **upstream toolkit repo** (e.g., `microsoft/finops-toolkit`)
5. **For RES modules**: Ask for the **resource provider** (e.g., `Microsoft.KeyVault/vaults`)

## Step 1: Determine Module Type

From the module path, determine the type and appropriate sources:

### PTN (Pattern) Modules
- **Source of Truth**: The official Microsoft toolkit repository
- **Examples**:
  - `avm/ptn/finops-toolkit/*` → `microsoft/finops-toolkit`
  - `avm/ptn/lz/*` → `Azure/Enterprise-Scale` or `Azure/terraform-azurerm-caf-enterprise-scale`
  - `avm/ptn/aca-lza/*` → `Azure/aca-landing-zone-accelerator`
  - `avm/ptn/ai-platform/*` → `Azure/aihub` or related AI repos
- **What to scan**: Upstream releases, breaking changes, new features to incorporate

### RES (Resource) Modules
- **Source of Truth**: Azure Resource Manager API specs
- **Documentation**: `https://learn.microsoft.com/en-us/azure/templates/{provider}/{resource}`
- **Examples**:
  - `avm/res/key-vault/vault` → `Microsoft.KeyVault/vaults`
  - `avm/res/storage/storage-account` → `Microsoft.Storage/storageAccounts`
- **What to scan**: New API versions, deprecated properties, new features

### UTL (Utility) Modules
- **Source of Truth**: AVM specifications only
- **What to scan**: AVM spec changes, usage patterns

## Step 2: Scan bicep-registry-modules

Search for issues and PRs in `Azure/bicep-registry-modules`:

### Issues Search
```
repo:Azure/bicep-registry-modules is:issue [module keywords]
repo:Azure/bicep-registry-modules is:issue [resource type]
repo:Azure/bicep-registry-modules is:issue label:"Type: AVM"
```

### PRs Search
```
repo:Azure/bicep-registry-modules is:pr [module keywords]
repo:Azure/bicep-registry-modules is:pr [module path]
```

Report:
```
## bicep-registry-modules Activity

### Open Issues Affecting Your Module
| Issue | Title | Labels | Updated |
|-------|-------|--------|---------|
| #123 | [Title] | [labels] | [date] |

### Recent PRs
| PR | Title | Status | Author | Updated |
|----|-------|--------|--------|---------|
| #456 | [Title] | Open/Merged | @user | [date] |

### ⚠️ Potential Impact
- [Description of any concerning changes]
```

## Step 3: Scan Upstream Source (Module-Type Specific)

### For PTN Modules: Scan Upstream Toolkit

Search the upstream toolkit repository for changes:

```
repo:[upstream-repo] is:issue
repo:[upstream-repo] is:pr
repo:[upstream-repo] release
```

Fetch recent releases:
`https://github.com/[upstream-repo]/releases`

Report:
```
## Upstream Toolkit Activity ([repo-name])

### Recent Releases
| Version | Date | Breaking Changes? |
|---------|------|-------------------|
| v1.2.0 | [date] | Yes/No |

### Open Issues in Upstream
| Issue | Title | Relevance |
|-------|-------|-----------|
| #123 | [Title] | [How it affects your PTN] |

### Changes to Incorporate
- [ ] Feature X added in v1.2.0 - consider adding to PTN
- [ ] Breaking change in v1.1.0 - update required
- [ ] Bug fix in v1.0.5 - already addressed / needs porting

### Sync Status
| Upstream Version | Your PTN Version | Sync Status |
|------------------|------------------|-------------|
| v1.2.0 | v1.0.0 | ⚠️ Behind - review changes |
```

### For RES Modules: Check Azure API Specs

Check the Azure Resource Manager documentation:

```
## Azure API Changes ([resource-provider])

Documentation: https://learn.microsoft.com/en-us/azure/templates/[provider]/[resource]

### Current API Version in Module
[Your current API version, e.g., 2023-01-01]

### Latest API Version Available
[Latest version from docs]

### New Properties in Latest API
| Property | Description | Breaking? |
|----------|-------------|-----------|
| [name] | [description] | Yes/No |

### Deprecated Properties
| Property | Deprecated In | Alternative |
|----------|---------------|-------------|
| [name] | [version] | [new property] |

### Recommended Action
- [ ] Update API version from X to Y
- [ ] Add support for new property Z
- [ ] Add deprecation warning for property W
```

## Step 4: Scan Azure-Verified-Modules

Search for specification changes in `Azure/Azure-Verified-Modules`:

```
repo:Azure/Azure-Verified-Modules [module keywords]
repo:Azure/Azure-Verified-Modules BCPNFR OR SNFR OR PTNFR
repo:Azure/Azure-Verified-Modules breaking change
```

Report:
```
## Specification Changes

### Recent Spec Updates
| Issue/PR | Description | Impact |
|----------|-------------|--------|
| #789 | [Spec change] | [High/Medium/Low] |

### Module Status
- Your module mentioned in issues: [Yes/No]
- Deprecation discussions: [Yes/No]
- New requirements affecting you: [Yes/No]
```

## Step 5: Check Dependencies

If the module uses other AVM modules, check their status:

```
## Dependency Health

| Dependency | Current | Latest | Status |
|------------|---------|--------|--------|
| avm/res/storage/storage-account | 0.9.0 | 0.11.0 | ⚠️ Update available |
| avm/res/network/private-endpoint | 0.4.0 | 0.4.0 | ✅ Current |

### Breaking Changes in Dependencies
- [List any breaking changes in newer versions]
```

## Step 6: Community Activity

Check for community discussions:

```
## Community Activity

### Discussions mentioning your module
- [Link to discussion if any]

### Feature requests related to your module
- [Relevant feature requests]

### People working on similar things
- @user1 - Working on [related feature]
- @user2 - Proposed [related change]
```

## Step 7: Summary & Recommendations

```
## Summary

### 🟢 Good News
- [Positive findings]

### 🟡 Monitor
- [Things to keep an eye on]

### 🔴 Action Needed
- [Things requiring immediate attention]

### Source-Specific Actions

**For PTN modules:**
- [ ] Sync with upstream release vX.Y
- [ ] Review upstream roadmap for upcoming changes
- [ ] Check if upstream deprecations affect your PTN

**For RES modules:**
- [ ] Update to latest API version
- [ ] Add new properties from API spec
- [ ] Add deprecation warnings for removed properties

### Recommended Actions
1. [First action]
2. [Second action]
3. [Third action]
```

## Step 8: Track with Todos

Use `#todos` if follow-up actions are needed:
- [ ] Review issue #X
- [ ] Sync with upstream vY
- [ ] Update dependency Z
- [ ] Coordinate with @user about W
