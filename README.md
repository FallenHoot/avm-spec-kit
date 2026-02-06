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

1. **Python 3.11+** - [Download](https://www.python.org/downloads/)
2. **Git** - [Download](https://git-scm.com/downloads)
3. **uv** (Python package manager) - See installation below
4. **An AI coding assistant** (GitHub Copilot, Claude, Cursor, etc.)

### Step 1: Install uv

Choose your platform:

<details>
<summary><strong>🪟 Windows</strong></summary>

**Option A: WinGet (Recommended)**
```powershell
winget install --id=astral-sh.uv -e
```

**Option B: Scoop**
```powershell
scoop install main/uv
```

**Option C: PowerShell installer**
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

After installation, restart your terminal or run:
```powershell
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")
```

</details>

<details>
<summary><strong>🍎 macOS</strong></summary>

**Option A: Homebrew (Recommended)**
```bash
brew install uv
```

**Option B: Shell installer**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

</details>

<details>
<summary><strong>🐧 Linux</strong></summary>

**Shell installer**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Or if you don't have curl:
```bash
wget -qO- https://astral.sh/uv/install.sh | sh
```

</details>

Verify installation:
```bash
uv --version
```

### Step 2: Install spec-kit CLI

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

> **Note:** If you see a PATH warning, follow the instructions or run `uv tool update-shell`

Verify installation:
```bash
specify check
```

### Step 3: Add AVM Spec-Kit to Your Project

**Option A: Copy just the constitution (minimal)**
```bash
# In your AVM module directory
mkdir -p .specify/memory
curl -o .specify/memory/constitution.md https://raw.githubusercontent.com/FallenHoot/avm-spec-kit/main/.specify/memory/constitution.md
echo ".specify/" >> .gitignore
```

**Option B: Clone and copy everything**
```bash
git clone https://github.com/FallenHoot/avm-spec-kit.git
cp -r avm-spec-kit/.specify/ your-avm-module/
echo ".specify/" >> your-avm-module/.gitignore
```

### Step 4: Use the Commands

Start your AI assistant and use these commands:
```
/avm.module      - Define a new AVM module specification
/avm.design      - Create architecture decisions and interfaces  
/avm.test        - Generate E2E test scenarios
/avm.implement   - Build the module following AVM patterns
/avm.contribute  - Prepare for contributing (check issues, owners, guidelines)
/avm.scan        - Scan repos for issues/PRs affecting your module
/avm.dependencies - Check module dependencies for updates
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

## Commands

### Development Commands

| Command | Purpose |
|---------|---------|
| `/avm.module` | Define a new AVM module specification |
| `/avm.design` | Create architecture decisions and interfaces |
| `/avm.test` | Generate E2E test scenarios |
| `/avm.implement` | Build the module following AVM patterns |

### Contribution Commands

| Command | Purpose |
|---------|---------|
| `/avm.contribute` | Prepare for contributing - reviews issues, identifies owners, checks guidelines |
| `/avm.scan` | Scan AVM repos for issues/PRs affecting your module |
| `/avm.dependencies` | Check module dependencies for updates and breaking changes |

### Example: Using `/avm.contribute`

```
/avm.contribute

Issue I'm addressing: https://github.com/Azure/bicep-registry-modules/issues/1234

My proposed changes:
Add support for customer-managed keys to the storage module

Please help me:
1. Review the issue comments and any linked PRs to check if someone else is already working on this
2. Identify the module/code owners I should coordinate with
3. Find any contribution guidelines, specs, or interface standards I need to follow
4. Flag potential scope creep - what's the minimum change needed?
5. Review my PR before I submit (when ready)
```

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

## Troubleshooting

### "uv: command not found" / "uv is not recognized"

Your PATH wasn't updated after installation. Try:

**Windows (PowerShell):**
```powershell
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")
```

**macOS/Linux:**
```bash
source ~/.bashrc  # or ~/.zshrc
```

Or simply restart your terminal.

### "specify: command not found"

The uv tools directory isn't in your PATH. Run:
```bash
uv tool update-shell
```
Then restart your terminal.

### Slash commands not appearing in AI assistant

1. Make sure `.specify/templates/commands/` exists in your project
2. Restart your IDE/editor completely
3. Check your AI agent supports slash commands (see [supported agents](https://github.com/github/spec-kit#-supported-ai-agents))

### Constitution customizations lost after upgrade

Back up before upgrading:
```bash
cp .specify/memory/constitution.md .specify/memory/constitution-backup.md
specify init --here --force --ai copilot
mv .specify/memory/constitution-backup.md .specify/memory/constitution.md
```

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
