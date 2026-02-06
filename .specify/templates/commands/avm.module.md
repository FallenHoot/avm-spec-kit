---
description: "Define a new AVM module specification"
tools: ["editFiles", "fetch", "get_bicep_best_practices", "azure_get_azure_verified_module", "todos"]
---

# AVM Module Specification

You are creating a specification for an Azure Verified Module. Follow the AVM constitution strictly.

## Step 1: Gather Context

Ask the user (if not already provided):
1. **Module type**: RES (single resource), PTN (pattern/multiple resources), or UTL (utility)?
2. **Language**: Bicep or Terraform?
3. **Resource/Pattern name**: What does this module deploy?
4. **Scope**: What is included/excluded?

## Step 2: Validate Against Constitution

Before proceeding, confirm:
- [ ] Module path follows convention: `avm/{res|ptn|utl}/<provider>/<resource>`
- [ ] Module type is appropriate for the scope
- [ ] No overlap with existing AVM modules (check with `#azure_get_azure_verified_module`)

## Step 3: Create Module Specification

Write the specification to `.specify/specs/<module-name>.md`:

```markdown
---
module: <module-path>
type: <RES|PTN|UTL>
language: <Bicep|Terraform>
version: <semantic-version>
status: draft
---

# Module: <module-name>

## Overview
<1-2 sentence description of what this module deploys>

## Scope

### In Scope
- <Resource 1>
- <Resource 2>

### Out of Scope
- <What this module does NOT do>

## Parameters

### Required
| Parameter | Type | Description |
|-----------|------|-------------|
| name | string | The name of the primary resource |

### Optional
| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|

## Outputs
| Output | Type | Description |
|--------|------|-------------|
| resourceId | string | The resource ID |
| name | string | The resource name |

## Test Scenarios

### defaults
- Deploy with only required parameters
- Validates: Basic deployment works

### waf-aligned  
- Deploy with production security settings
- Validates: Module supports enterprise requirements

## Dependencies
- <List any AVM modules this depends on>

## Breaking Changes from Previous Version
- <List any breaking changes if this is an update>
```

## Step 4: Track Progress

Use `#todos` to create actionable tasks:
- [ ] Module specification created
- [ ] Parameters defined
- [ ] Test scenarios planned
- [ ] Ready for /avm.design
