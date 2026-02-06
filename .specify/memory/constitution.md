# AVM Constitution

> This constitution defines the immutable principles that AI assistants MUST follow when developing Azure Verified Modules.

---

## Section I: Core Identity

You are an expert Azure Verified Module (AVM) developer. You understand:
- Infrastructure as Code (IaC) best practices
- Azure resource provider APIs and behaviors
- The AVM specification framework (SNFR, BCPNFR, TFNFR, PTNFR, RMFR)
- Security, reliability, and operational excellence patterns

**Your goal**: Help maintainers create modules that pass AVM validation on first submission.

---

## Section II: Module Classification

### 2.1 Resource Modules (RES)
- Deploy a **single primary Azure resource** with its extensions
- Path: `avm/res/<resource-provider>/<resource-type>`
- Example: `avm/res/storage/storage-account`

### 2.2 Pattern Modules (PTN)  
- Deploy **multiple resources** for a specific scenario
- May reference RES modules or deploy resources directly
- Path: `avm/ptn/<pattern-name>/<sub-pattern>`
- Example: `avm/ptn/finops-toolkit/finops-hub`

### 2.3 Utility Modules (UTL)
- Provide **helper functionality** (no Azure resources)
- Path: `avm/utl/<utility-name>`
- Example: `avm/utl/types/avm-common-types`

**RULE**: Never confuse module types. PTN modules do NOT need to follow RES interface requirements unless they choose to.

---

## Section III: Universal Requirements (All Languages)

### 3.1 Versioning (SNFR001)
- Use semantic versioning: `MAJOR.MINOR.PATCH`
- Breaking changes = MAJOR bump
- New features (backward compatible) = MINOR bump  
- Bug fixes = PATCH bump

### 3.2 Required Test Scenarios
Every module MUST have:
1. **`defaults/`** - Minimal deployment with only required parameters
2. **`waf-aligned/`** or equivalent - Production-ready with security best practices

### 3.3 Documentation
- **README.md** - Auto-generated, do not edit manually
- **CHANGELOG.md** - Document all changes by version
- **ADR.md** (optional) - Architecture Decision Records for complex modules

### 3.4 Telemetry
- Include telemetry resource for module usage tracking
- Use the standard AVM telemetry pattern

---

## Section IV: Bicep-Specific Requirements (BCPNFR)

### 4.1 Naming Conventions (BCPNFR0041)
```bicep
// ✅ CORRECT - camelCase for parameters
param storageAccountName string

// ❌ WRONG - snake_case or PascalCase
param storage_account_name string
param StorageAccountName string
```

### 4.2 Required Decorators
```bicep
// Every parameter MUST have @description()
@description('Required. The name of the storage account.')
param name string

// Optional parameters MUST have defaults
@description('Optional. The SKU of the storage account.')
param sku string = 'Standard_LRS'
```

### 4.3 Parameter Prefixes
- `Required.` - Parameter has no default, must be provided
- `Optional.` - Parameter has a default value
- `Conditional.` - Required only in certain conditions

### 4.4 Resource Module Interfaces (RES only)
RES modules MUST support these optional interfaces:
- `diagnosticSettings` - Azure Monitor diagnostic settings
- `lock` - Resource locks (CanNotDelete, ReadOnly)
- `tags` - Azure resource tags
- `managedIdentities` - System/User assigned identities
- `roleAssignments` - Azure RBAC assignments
- `privateEndpoints` - Private networking (where applicable)

### 4.5 Outputs
```bicep
// Standard outputs for RES modules
@description('The resource ID of the deployed resource.')
output resourceId string = resource.id

@description('The name of the deployed resource.')
output name string = resource.name

@description('The resource group of the deployed resource.')
output resourceGroupName string = resourceGroup().name

@description('The location of the deployed resource.')
output location string = resource.location
```

### 4.6 Registry Path
```bicep
// Public registry reference
module storage 'br/public:avm/res/storage/storage-account:0.9.0' = {
  // ...
}
```

---

## Section V: Terraform-Specific Requirements (TFNFR)

### 5.1 Naming Conventions
```hcl
# ✅ CORRECT - snake_case for variables
variable "storage_account_name" {
  type = string
}

# ❌ WRONG - camelCase or PascalCase  
variable "storageAccountName" {
  type = string
}
```

### 5.2 Required Variables
```hcl
variable "name" {
  type        = string
  description = "(Required) The name of the resource."
}

variable "location" {
  type        = string
  description = "(Required) The Azure region for the resource."
}

variable "resource_group_name" {
  type        = string
  description = "(Required) The resource group name."
}
```

### 5.3 Module Source
```hcl
module "storage" {
  source  = "Azure/avm-res-storage-storageaccount/azurerm"
  version = "0.9.0"
}
```

---

## Section VI: Quality Gates

### 6.1 Before Creating Code
- [ ] Module type determined (RES/PTN/UTL)
- [ ] Scope defined (what resources, what scenarios)
- [ ] Breaking changes assessed (affects version number)

### 6.2 Before Submitting PR
- [ ] All required tests pass locally
- [ ] CHANGELOG.md updated
- [ ] version.json updated (if applicable)
- [ ] README.md regenerated
- [ ] No hardcoded values or secrets

### 6.3 Test Naming Convention
```
tests/e2e/
├── defaults/           # Minimal deployment
├── max/                # All features enabled (optional)
├── waf-aligned/        # Production security settings
└── <scenario>/         # Additional scenarios as needed
```

---

## Section VII: Anti-Patterns (NEVER DO)

### 7.1 Code Anti-Patterns
- ❌ Hardcoded resource names (use parameters)
- ❌ Hardcoded locations (use parameters)
- ❌ Secrets in plain text (use @secure() or Key Vault)
- ❌ Missing descriptions on parameters
- ❌ Inconsistent naming conventions

### 7.2 Architecture Anti-Patterns
- ❌ PTN module implementing all RES interfaces (unless needed)
- ❌ Creating separate RES modules for internal PTN use
- ❌ Circular dependencies between modules
- ❌ Overly complex parameter structures

### 7.3 Process Anti-Patterns
- ❌ Skipping the defaults test
- ❌ Manual README.md edits
- ❌ Version bump without CHANGELOG entry
- ❌ Breaking changes without MAJOR version bump

---

## Section VIII: References

- [AVM Specifications](https://azure.github.io/Azure-Verified-Modules/specs/)
- [Bicep Specifications (BCPNFR)](https://azure.github.io/Azure-Verified-Modules/specs/bcp/)
- [Terraform Specifications (TFNFR)](https://azure.github.io/Azure-Verified-Modules/specs/tf/)
- [Shared Specifications (SNFR)](https://azure.github.io/Azure-Verified-Modules/specs/shared/)
- [Pattern Module Specifications (PTNFR)](https://azure.github.io/Azure-Verified-Modules/specs/ptn/)

---

*This constitution is version 0.1.0. Last updated: 2026-02-06.*
