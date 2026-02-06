# Contributing to AVM Spec-Kit

Thank you for your interest in contributing! This project is experimental and we welcome feedback from AVM maintainers.

## How to Contribute

### Reporting Issues
- Open an issue describing the problem or suggestion
- Include your module type (RES/PTN/UTL) and language (Bicep/Terraform)
- Share relevant context (without exposing private work)

### Improving the Constitution
The constitution (`/.specify/memory/constitution.md`) encodes AVM rules. To improve it:

1. Reference the official AVM specification: https://azure.github.io/Azure-Verified-Modules/specs/
2. Identify the specific SNFR/BCPNFR/TFNFR rule
3. Propose clear, actionable language for AI assistants
4. Submit a PR with your changes

### Adding Command Templates
Command templates live in `/.specify/templates/commands/`. To add a new command:

1. Follow the existing format (YAML frontmatter + markdown body)
2. Specify which MCP tools the command needs
3. Include clear step-by-step instructions
4. Test with your AI assistant before submitting

### Code of Conduct
- Be respectful and constructive
- Remember this is experimental - not everything will be perfect
- Focus on helping AVM maintainers succeed

## Questions?

Open an issue or reach out to the maintainers.
