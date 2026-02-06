# 🧪 AVM Spec-Kit

> **Experimental** - A [spec-kit](https://github.com/github/spec-kit) extension for Azure Verified Module (AVM) maintainers, contributors, and editors.

[![Built with Spec-Kit](https://img.shields.io/badge/Built%20with-Spec--Kit-blue)](https://github.com/github/spec-kit)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Status: Experimental](https://img.shields.io/badge/Status-Experimental-orange)]()

## What is this?

AVM Spec-Kit provides AI-assisted guardrails for developing [Azure Verified Modules](https://aka.ms/AVM). It encodes AVM specifications (BCPNFR for Bicep, TFNFR for Terraform) as a "constitution" that AI coding assistants follow.

**This helps you:**
- Maintain consistency with AVM specifications
- Reduce context loss in long AI conversations
- Scaffold modules that pass AVM validation on first try
- Keep your development process private while producing polished outputs

## ⚠️ Experimental Status

This project is an experiment by AVM community members. It is:
- **NOT** an official Microsoft or AVM project
- **NOT** endorsed by the AVM core team (yet)
- A proof-of-concept to explore AI-assisted IaC development

We welcome feedback from other AVM maintainers!

## Supported Languages

| Language | Spec Reference | Status |
|----------|---------------|--------|
| Bicep | [BCPNFR](https://azure.github.io/Azure-Verified-Modules/specs/bcp/) | ✅ Supported |
| Terraform | [TFNFR](https://azure.github.io/Azure-Verified-Modules/specs/tf/) | 🚧 Planned |

## Quick Start

### Prerequisites
- [spec-kit CLI](https://github.com/github/spec-kit) installed
- An AI coding assistant (GitHub Copilot, Claude, Cursor, etc.)

### Installation

1. **Clone this repo** (or copy the `.specify/` folder):
   ```bash
   git clone https://github.com/FallenHoot/avm-spec-kit.git
   ```

2. **Copy to your AVM module project**:
   ```bash
   cp -r avm-spec-kit/.specify/ your-avm-module/
   ```

3. **Add to .gitignore** (keep your process private):
   ```bash
   echo ".specify/" >> your-avm-module/.gitignore
   ```

4. **Start your AI assistant** and use the commands:
   ```
   /avm.module    - Define a new AVM module specification
   /avm.design    - Create architecture decisions and interfaces  
   /avm.test      - Generate E2E test scenarios
   /avm.implement - Build the module following AVM patterns
   ```

## How It Works

```
┌─────────────────────────────────────────────────────────────┐
│  Your AVM Module Repo                                        │
├─────────────────────────────────────────────────────────────┤
│  .specify/                    ← .gitignore (PRIVATE)        │
│  ├── memory/                                                 │
│  │   ├── constitution.md      ← AVM rules AI must follow    │
│  │   └── session.md           ← Your notes (optional)       │
│  ├── specs/                                                  │
│  │   └── my-module.md         ← Current work in progress    │
│  └── templates/                                              │
│       └── commands/           ← /avm.* slash commands       │
├─────────────────────────────────────────────────────────────┤
│  avm/ptn/my-module/           ← Actual code (PUBLIC)        │
│  ├── main.bicep                                              │
│  ├── README.md                                               │
│  └── tests/e2e/                                              │
└─────────────────────────────────────────────────────────────┘
```

**Key principle**: Your learning process stays private. Your polished output goes public.

## What's in the Constitution?

The constitution encodes AVM specifications as AI guardrails:

### Universal Rules (All Languages)
- Semantic versioning requirements
- Required test scenarios (defaults, WAF-aligned)
- Module classification (RES, PTN, UTL)
- Interface requirements (diagnostics, locks, tags, RBAC)

### Bicep-Specific (BCPNFR)
- Parameter naming: camelCase
- Registry path: `br/public:avm/res|ptn/...`
- Required decorators: `@description()`, `@metadata()`
- Output conventions

### Terraform-Specific (TFNFR)
- Variable naming: snake_case
- Module source: `Azure/avm-res-.../azurerm`
- Required variables and outputs

## Privacy Model

| What | Where | Who Sees |
|------|-------|----------|
| Your AI conversations | Chat tool (Copilot/Claude) | **No one** |
| Your experiments | `.specify/` (gitignored) | **Only you** |
| Your mistakes | Local branch | **Only you** |
| Clean final code | Git commits | **Your peers** |

## Contributing

This is an experiment! We welcome:
- Feedback from AVM maintainers
- Constitution improvements
- New command templates
- Bug reports

Please open an issue or PR.

## Related Projects

- [spec-kit](https://github.com/github/spec-kit) - The foundation this builds on
- [Azure Verified Modules](https://aka.ms/AVM) - The official AVM program
- [bicep-registry-modules](https://github.com/Azure/bicep-registry-modules) - AVM Bicep modules
- [Bicep MCP Server](https://github.com/Azure/bicep) - AI tools for Bicep

## License

MIT License - see [LICENSE](LICENSE)

## Acknowledgments

- [GitHub Spec-Kit team](https://github.com/github/spec-kit) for the foundation
- [AVM Core Team](https://azure.github.io/Azure-Verified-Modules/) for the specifications
- [@johnlokerse](https://github.com/johnlokerse) for Bicep + AI integration patterns
