---
description: "Build the AVM module following specifications and AVM patterns"
tools: ["editFiles", "runCommands", "terminalLastCommand", "get_bicep_best_practices", "bicepschema", "azure_get_azure_verified_module", "todos"]
---

# AVM Module Implementation

You are implementing an Azure Verified Module based on the specification and design.

## Prerequisites

- Module specification: `.specify/specs/<module-name>.md`
- Design complete: `/avm.design`
- Tests defined: `/avm.test`
- Constitution loaded: `.specify/memory/constitution.md`

## Step 1: Load Context

Read all planning documents:
1. `.specify/specs/<module-name>.md` - What to build
2. `.specify/memory/constitution.md` - Rules to follow
3. Existing test files - Expected behavior

## Step 2: File Structure

Create the standard AVM structure:

### Bicep Module Structure
```
avm/<type>/<provider>/<resource>/
├── main.bicep                 # Main module entry point
├── main.json                  # Generated - DO NOT EDIT
├── README.md                  # Generated - DO NOT EDIT
├── version.json               # Module version
├── CHANGELOG.md               # Version history
├── modules/                   # Child/helper modules (if needed)
│   └── <child>.bicep
└── tests/
    └── e2e/
        ├── defaults/
        │   └── main.test.bicep
        └── waf-aligned/
            └── main.test.bicep
```

## Step 3: Implement main.bicep

Follow this structure:

```bicep
metadata name = '<Module Name>'
metadata description = '<Module Description>'
metadata owner = 'Azure/avm-ptn-<team>'  // or avm-res-<team>

// ============== //
//   Parameters   //
// ============== //

@description('Required. The name of the resource.')
param name string

@description('Required. Location for all resources.')
param location string

@description('Optional. Tags for all resources.')
param tags object = {}

@description('Optional. Enable telemetry.')
param enableTelemetry bool = true

// ... additional parameters following naming conventions

// ============== //
//   Variables    //
// ============== //

var builtInRoleNames = {
  // Role definitions if using roleAssignments
}

// ============== //
//   Resources    //
// ============== //

// Telemetry (required for AVM)
#disable-next-line no-deployments-resources
resource avmTelemetry 'Microsoft.Resources/deployments@2023-07-01' = if (enableTelemetry) {
  name: '46d3xbcp.ptn.<module-name>.${replace('-..--..-', '.', '-')}.${substring(uniqueString(deployment().name, location), 0, 4)}'
  properties: {
    mode: 'Incremental'
    template: {
      '$schema': 'https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#'
      contentVersion: '1.0.0.0'
      resources: []
    }
  }
}

// Main resource(s)
resource mainResource '<resource-type>@<api-version>' = {
  name: name
  location: location
  tags: tags
  properties: {
    // ...
  }
}

// ============== //
//    Outputs     //
// ============== //

@description('The resource ID of the deployed resource.')
output resourceId string = mainResource.id

@description('The name of the deployed resource.')
output name string = mainResource.name

@description('The resource group of the deployed resource.')
output resourceGroupName string = resourceGroup().name

@description('The location of the deployed resource.')
output location string = mainResource.location
```

## Step 4: Implement version.json

```json
{
  "$schema": "https://aka.ms/bicep-registry-module-version-file-schema#",
  "version": "1.0",
  "pathFilters": [
    "./main.bicep",
    "./main.json",
    "./modules/**"
  ]
}
```

## Step 5: Validate Implementation

Run these checks after implementation:

```bash
# 1. Validate Bicep syntax
bicep build main.bicep

# 2. Lint the module
bicep lint main.bicep

# 3. Format the code
bicep format main.bicep

# 4. Build all test files
bicep build tests/e2e/defaults/main.test.bicep
bicep build tests/e2e/waf-aligned/main.test.bicep
```

Use `#runCommands` to execute these and `#terminalLastCommand` to check for errors.

## Step 6: Quality Checklist

Before considering implementation complete:

- [ ] All parameters have `@description()` decorator
- [ ] Required vs Optional prefix in descriptions
- [ ] camelCase naming for all parameters (Bicep)
- [ ] Telemetry resource included
- [ ] Standard outputs present
- [ ] `bicep build` passes with no errors
- [ ] `bicep lint` passes with no warnings
- [ ] All test files build successfully
- [ ] CHANGELOG.md updated
- [ ] version.json correct

## Step 7: Generate README

The README is auto-generated. To update:

```bash
# This is typically done by CI, but you can preview locally
# Using AVM tooling if available
```

## Step 8: Track Completion

Use `#todos`:
- [ ] main.bicep implemented
- [ ] All parameters follow conventions
- [ ] version.json created
- [ ] CHANGELOG.md updated
- [ ] bicep build passes
- [ ] bicep lint passes  
- [ ] All E2E tests build
- [ ] Ready for PR submission
