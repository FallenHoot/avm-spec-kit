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

## Step 1: Extract Search Context

From the module path, determine:
- Module type: RES, PTN, or UTL
- Resource provider (if RES)
- Pattern name (if PTN)
- Related Azure services

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

## Step 3: Scan Azure-Verified-Modules

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

## Step 4: Check Dependencies

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

## Step 5: Community Activity

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

## Step 6: Summary & Recommendations

```
## Summary

### 🟢 Good News
- [Positive findings]

### 🟡 Monitor
- [Things to keep an eye on]

### 🔴 Action Needed
- [Things requiring immediate attention]

### Recommended Actions
1. [First action]
2. [Second action]
3. [Third action]
```

## Step 7: Track with Todos

Use `#todos` if follow-up actions are needed:
- [ ] Review issue #X
- [ ] Update dependency Y
- [ ] Coordinate with @user about Z
