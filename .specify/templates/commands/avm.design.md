---
description: "Create architecture decisions and interface design for an AVM module"
tools: ["editFiles", "fetch", "get_bicep_best_practices", "bicepschema", "azure_get_azure_verified_module", "todos"]
---

# AVM Module Design

You are designing the architecture and interfaces for an Azure Verified Module. Reference the module specification and AVM constitution.

## Prerequisites

- Module specification exists (created via `/avm.module`)
- Read `.specify/specs/<module-name>.md` for context
- Read `.specify/memory/constitution.md` for rules

## Step 1: Determine Interfaces (RES modules)

For **Resource Modules (RES)**, determine which standard interfaces apply:

| Interface | Required? | Notes |
|-----------|-----------|-------|
| diagnosticSettings | Check if resource supports diagnostics | |
| lock | Yes (all resources) | CanNotDelete, ReadOnly |
| tags | Yes (all resources) | |
| managedIdentities | Check if resource supports MI | System, UserAssigned |
| roleAssignments | Yes (all resources) | RBAC assignments |
| privateEndpoints | Check if resource supports PE | |
| customerManagedKey | Check if resource supports CMK | |

For **Pattern Modules (PTN)**, interfaces are optional - only implement what the pattern needs.

## Step 2: Architecture Decision Record

If the module has non-obvious design decisions, create an ADR:

```markdown
# Architecture Decision Record

## ADR-001: <Decision Title>

### Status
Accepted | Proposed | Deprecated

### Context
<Why is this decision needed?>

### Decision
<What was decided?>

### Consequences
<What are the implications?>

### Alternatives Considered
<What else was considered and why rejected?>
```

## Step 3: Design Parameters

Follow naming conventions from constitution:

### Bicep
```bicep
// Required parameters (no default)
@description('Required. The name of the resource.')
param name string

// Optional parameters (with default)
@description('Optional. The SKU name.')
param skuName string = 'Standard'

// Conditional parameters
@description('Conditional. Required when X is enabled.')
param conditionalParam string = ''
```

### Terraform
```hcl
variable "name" {
  type        = string
  description = "(Required) The name of the resource."
}

variable "sku_name" {
  type        = string
  default     = "Standard"
  description = "(Optional) The SKU name. Defaults to Standard."
}
```

## Step 4: Design Outputs

### Bicep
```bicep
@description('The resource ID.')
output resourceId string = resource.id

@description('The resource name.')
output name string = resource.name

@description('The resource group name.')
output resourceGroupName string = resourceGroup().name
```

### Terraform
```hcl
output "resource_id" {
  value       = azurerm_resource.this.id
  description = "The resource ID."
}
```

## Step 5: Identify Dependencies

Check if you should use existing AVM modules:
- Use `#azure_get_azure_verified_module` to find available modules
- Prefer AVM modules over raw resource declarations
- Document why if NOT using an available AVM module

## Step 6: Update Specification

Add design decisions to `.specify/specs/<module-name>.md`:
- Parameter details with types and validation
- Interface decisions
- Dependency choices
- Any ADRs created

## Step 7: Track Progress

Use `#todos`:
- [ ] Interfaces determined
- [ ] Parameters designed with proper naming
- [ ] Outputs defined
- [ ] Dependencies identified
- [ ] ADRs created (if needed)
- [ ] Ready for /avm.test
