# Integration Examples Reference System

This directory contains consolidated, reusable integration examples for all sub-agent commands. Instead of duplicating integration code across individual command files, use these standardized templates.

## Available Templates

### 📝 [VS Code Integration](./vscode-integration.md)
- Command definitions for VS Code extensions
- Task configurations for tasks.json
- Keybinding suggestions
- Interactive and non-interactive task variants

### ⚙️ [CI/CD Integration](./cicd-integration.md)
- GitHub Actions workflows
- GitLab CI pipeline configurations
- Jenkins pipeline scripts
- Azure DevOps configurations
- Platform-specific examples for each command

### 🎣 [Git Hooks Integration](./git-hooks-integration.md)
- Pre-commit hooks for quality checks
- Pre-push hooks for architecture validation
- Post-commit hooks for documentation and metrics
- Installation and management scripts

## How to Use in Command Files

### Method 1: Direct Reference
Instead of duplicating examples, reference the shared templates:

```markdown
## Integration

See [shared integration examples](../shared/examples/) for:
- [VS Code setup](../shared/examples/vscode-integration.md#tech-debt-finder-integration)
- [CI/CD pipelines](../shared/examples/cicd-integration.md#github-actions---tech-debt-finder)
- [Git hooks](../shared/examples/git-hooks-integration.md#tech-debt-finder-pre-commit)
```

### Method 2: Parameterized Include
Use command-specific parameters with the templates:

```markdown
## VS Code Integration

Use the [VS Code task template](../shared/examples/vscode-integration.md#task-definition-template) with these parameters:
- **COMMAND_NAME**: `sub-agent-tech-debt-finder-fixer`
- **TASK_LABEL**: `Find and Fix Tech Debt`
- **DEFAULT_ARGS**: `--dry-run`
- **GROUP**: `build`
```

### Method 3: Command-Specific Examples
For unique integrations, create command-specific examples that extend the base templates:

```markdown
## Integration

### Quick Start
See [basic VS Code integration](../shared/examples/vscode-integration.md) for standard setup.

### Advanced Configuration
For tech debt finder specific features:
\```json
{
  "label": "Find Tech Debt with Auto-fix",
  "command": "sub-agent-tech-debt-finder-fixer --auto-fix safe",
  "group": "build"
}
\```
```

## Template Parameters

### Common Parameters
All templates use these standard parameters that should be replaced with command-specific values:

- `{{COMMAND_NAME}}` - The actual command name (e.g., `sub-agent-tech-debt-finder-fixer`)
- `{{COMMAND_ID}}` - VS Code command identifier (e.g., `techDebtFinder`)
- `{{COMMAND_TITLE}}` - Human-readable title (e.g., `Find and Fix Tech Debt`)
- `{{ARGS}}` - Command arguments (e.g., `--dry-run`, `--interactive`)
- `{{JOB_NAME}}` - CI/CD job name (e.g., `tech-debt-check`)
- `{{HOOK_TYPE}}` - Git hook type (e.g., `pre-commit`, `pre-push`)

### Command-Specific Parameter Sets

#### Tech Debt Finder & Fixer
```
COMMAND_NAME: sub-agent-tech-debt-finder-fixer
COMMAND_ID: techDebtFinder
COMMAND_TITLE: Find and Fix Tech Debt
DEFAULT_ARGS: --dry-run
SCORE_FILE: tech-debt-score.txt
```

#### Architecture Reviewer
```
COMMAND_NAME: sub-agent-architecture-reviewer
COMMAND_ID: architectureReview
COMMAND_TITLE: Review Architecture
DEFAULT_ARGS: --detect-violations
REPORT_PATHS: architecture-report.md, diagrams/
```

#### Test Generator
```
COMMAND_NAME: sub-agent-test-generator
COMMAND_ID: testGenerator
COMMAND_TITLE: Generate Tests
DEFAULT_ARGS: --only-untested
REPORT_PATHS: coverage/, test-reports/
```

#### Performance Optimizer
```
COMMAND_NAME: sub-agent-performance-optimizer
COMMAND_ID: performanceOptimizer
COMMAND_TITLE: Optimize Performance
DEFAULT_ARGS: --target all --dry-run
SCORE_FILE: performance-score.txt
```

#### Refactor
```
COMMAND_NAME: sub-agent-refactor
COMMAND_ID: refactor
COMMAND_TITLE: Refactor Code
DEFAULT_ARGS: src/ --dry-run
EXIT_CONDITION: if [ $? -ne 0 ]; then echo "Refactoring opportunities found"; exit 1; fi
```

#### New Feature Builder
```
COMMAND_NAME: sub-agent-new-feature
COMMAND_ID: newFeature
COMMAND_TITLE: Build New Feature
DEFAULT_ARGS: --review-mode strict --test-first
REQUIRES_INPUT: true
```

#### Migration Assistant
```
COMMAND_NAME: sub-agent-migration-assistant
COMMAND_ID: migrationAssistant
COMMAND_TITLE: Migration Assistant
DEFAULT_ARGS: --check-compatibility
REPORT_PATHS: migration-report.md, compatibility-matrix.json
```

## Best Practices

### For Template Maintainers
1. **Keep templates generic** - Use parameters for command-specific values
2. **Provide multiple variants** - Basic, advanced, and specialized configurations
3. **Include error handling** - Proper exit codes and error messages
4. **Document parameters** - Clear parameter descriptions and examples
5. **Version control** - Track changes to templates for backward compatibility

### For Command Authors
1. **Reference shared templates** instead of duplicating code
2. **Use appropriate parameters** for your specific command
3. **Extend templates** only when necessary for unique features
4. **Test integrations** with provided parameters
5. **Contribute improvements** back to shared templates

### For Documentation
1. **Link to specific sections** of shared templates
2. **Provide parameter values** for your command
3. **Show working examples** with actual parameters filled in
4. **Keep command docs focused** on command-specific features
5. **Update references** when template structure changes

## Migration Guide

### For Existing Commands
To migrate existing integration examples to use shared templates:

1. **Identify duplicated content** in your command file
2. **Find equivalent template** in shared examples
3. **Extract parameters** specific to your command
4. **Replace duplicated content** with references to shared templates
5. **Add command-specific parameters** table
6. **Test all integration examples** work correctly

### Example Migration

**Before (duplicated content):**
```markdown
### VS Code Integration
\```json
{
  "command": "claude.techDebtFinder",
  "title": "Find and Fix Tech Debt"
}
\```

### CI/CD Pipeline
\```yaml
tech-debt-check:
  script:
    - sub-agent-tech-debt-finder-fixer --dry-run
\```
```

**After (using shared templates):**
```markdown
### Integration Examples

See [shared integration examples](../shared/examples/) for complete setup guides:

- **VS Code**: [Task configuration](../shared/examples/vscode-integration.md#tech-debt-finder-integration)
- **CI/CD**: [Pipeline examples](../shared/examples/cicd-integration.md#github-actions---tech-debt-finder)
- **Git Hooks**: [Pre-commit setup](../shared/examples/git-hooks-integration.md#tech-debt-finder-pre-commit)

#### Parameters for Tech Debt Finder
- **COMMAND_NAME**: `sub-agent-tech-debt-finder-fixer`
- **COMMAND_ID**: `techDebtFinder`
- **DEFAULT_ARGS**: `--dry-run`
```

## Template Versioning

Templates follow semantic versioning:
- **Major** (1.0.0): Breaking changes to template structure
- **Minor** (1.1.0): New templates or significant enhancements
- **Patch** (1.0.1): Bug fixes and minor improvements

Current version: **1.0.0**

## Contributing

To contribute improvements to shared templates:

1. **Identify common patterns** across multiple commands
2. **Create parameterized templates** that work for all use cases
3. **Test with multiple commands** to ensure compatibility
4. **Update documentation** with new parameters and examples
5. **Submit pull request** with clear description of changes

## Support

For questions about using shared templates:
1. Check existing template documentation
2. Review parameter examples for similar commands
3. Open an issue with specific integration questions
4. Contribute improvements for unclear documentation