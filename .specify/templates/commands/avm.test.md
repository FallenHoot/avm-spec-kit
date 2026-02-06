---
description: "Generate E2E test scenarios for an AVM module"
tools: ["editFiles", "runCommands", "get_bicep_best_practices", "todos"]
---

# AVM Test Scenario Generation

You are creating end-to-end test scenarios for an Azure Verified Module.

## Prerequisites

- Module specification exists (`.specify/specs/<module-name>.md`)
- Module design complete (`/avm.design`)
- Read `.specify/memory/constitution.md` for test requirements

## Step 1: Required Test Scenarios

Every AVM module MUST have these tests:

### 1. defaults/
**Purpose**: Validate minimal deployment with only required parameters

```bicep
// tests/e2e/defaults/main.test.bicep
metadata name = 'defaults'
metadata description = 'Minimal deployment with only required parameters.'

// Only required parameters - NO optional parameters
module testDeployment '../../../main.bicep' = {
  name: '${uniqueString(deployment().name)}-test'
  params: {
    name: 'min${uniqueString(resourceGroup().id)}'
    // Only other REQUIRED parameters
  }
}
```

### 2. waf-aligned/ (or security-focused scenario)
**Purpose**: Validate production-ready deployment with security best practices

```bicep
// tests/e2e/waf-aligned/main.test.bicep
metadata name = 'waf-aligned'
metadata description = 'WAF-aligned deployment with security best practices.'

module testDeployment '../../../main.bicep' = {
  name: '${uniqueString(deployment().name)}-test'
  params: {
    name: 'waf${uniqueString(resourceGroup().id)}'
    // Enable all security features
    // - Managed identity (if supported)
    // - Encryption (if supported)  
    // - Network restrictions (if supported)
    // - Diagnostic settings
    // - Locks
  }
}
```

## Step 2: Additional Test Scenarios

Based on module features, consider:

| Scenario | When to Include |
|----------|-----------------|
| `max/` | Complex modules with many optional features |
| `private-endpoint/` | Resources supporting private networking |
| `cmk/` | Resources supporting customer-managed keys |
| `<feature>/` | Specific feature validation |

## Step 3: Test File Structure

```
tests/e2e/
├── defaults/
│   ├── main.test.bicep
│   └── dependencies.bicep    # Shared dependencies (if needed)
├── waf-aligned/
│   ├── main.test.bicep
│   └── dependencies.bicep
└── <additional-scenarios>/
```

## Step 4: Test Metadata

Every test file MUST include:

```bicep
metadata name = '<test-name>'
metadata description = '<What this test validates>'
```

## Step 5: Dependencies Pattern

If tests need shared resources (VNet, Key Vault, etc.):

```bicep
// dependencies.bicep
@description('The location for the dependency resources.')
param location string = resourceGroup().location

// Deploy dependencies
resource vnet 'Microsoft.Network/virtualNetworks@2023-04-01' = {
  // ...
}

// Output references for main test
output vnetId string = vnet.id
```

```bicep
// main.test.bicep
module dependencies './dependencies.bicep' = {
  name: 'dependencies'
  params: {
    location: location
  }
}

module testDeployment '../../../main.bicep' = {
  name: 'testDeployment'
  params: {
    // Reference dependency outputs
    vnetId: dependencies.outputs.vnetId
  }
  dependsOn: [dependencies]
}
```

## Step 6: Validate Locally

Before submitting, run tests locally:

```bash
# Validate Bicep syntax
bicep build tests/e2e/defaults/main.test.bicep

# Deploy to test subscription (if you have access)
az deployment group create \
  --resource-group <test-rg> \
  --template-file tests/e2e/defaults/main.test.bicep
```

## Step 7: Track Progress

Use `#todos`:
- [ ] defaults/ test created
- [ ] waf-aligned/ test created  
- [ ] Additional scenarios identified and created
- [ ] Tests validated locally (bicep build)
- [ ] Ready for /avm.implement
