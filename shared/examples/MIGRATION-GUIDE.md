# Migration Guide: Converting to Shared Integration Examples

This guide shows how to migrate existing command files to use the consolidated integration examples instead of duplicated content.

## Overview

The migration involves:
1. **Identifying duplicated integration sections** in command files
2. **Replacing them with references** to shared templates
3. **Providing command-specific parameters** for the templates
4. **Testing that all examples still work** correctly

## Identified Duplications

Based on analysis of the codebase, these integration examples are duplicated across multiple files:

### VS Code Integration (4+ files affected)
- `sub-agent-tech-debt-finder-fixer.md`
- `sub-agent-architecture-reviewer.md`
- `sub-agent-refactor.md`
- `README.md`

### CI/CD Pipeline Integration (6+ files affected)
- `sub-agent-tech-debt-finder-fixer.md`
- `sub-agent-architecture-reviewer.md`
- `sub-agent-new-feature.md`
- `sub-agent-refactor.md`
- `README.md`
- `SETUP.md`

### Git Hooks Integration (3+ files affected)
- `sub-agent-tech-debt-finder-fixer.md`
- `sub-agent-refactor.md`
- `README.md`

## Step-by-Step Migration Examples

### Example 1: Tech Debt Finder & Fixer

**Current duplicated content in `sub-agent-tech-debt-finder-fixer.md`:**

```markdown
## Integration

### VS Code Command
\```json
{
  "command": "claude.techDebtFinder",
  "title": "Find and Fix Tech Debt"
}
\```

### Git Hook
\```bash
#!/bin/sh
# Pre-commit tech debt check
sub-agent-tech-debt-finder-fixer --dry-run
if [ $? -ne 0 ]; then
    echo "Tech debt found. Fix before committing."
    exit 1
fi
\```

### CI/CD Pipeline
\```yaml
tech-debt-check:
  script:
    - sub-agent-tech-debt-finder-fixer --dry-run
    - echo "Tech debt score: $(cat tech-debt-score.txt)"
  only:
    - merge_requests
\```
```

**Migrated content:**

```markdown
## Integration Examples

For complete integration setup, see [shared integration examples](../shared/examples/):

- **[VS Code Integration](../shared/examples/vscode-integration.md#tech-debt-finder-integration)** - Command definitions and task configurations
- **[CI/CD Pipelines](../shared/examples/cicd-integration.md#tech-debt-finder--fixer)** - GitHub Actions, GitLab CI, Jenkins examples  
- **[Git Hooks](../shared/examples/git-hooks-integration.md#tech-debt-finder-pre-commit)** - Pre-commit and pre-push hooks

### Command-Specific Parameters

Use these parameters with the shared templates:

| Parameter | Value |
|-----------|-------|
| `COMMAND_NAME` | `sub-agent-tech-debt-finder-fixer` |
| `COMMAND_ID` | `techDebtFinder` |
| `COMMAND_TITLE` | `Find and Fix Tech Debt` |
| `DEFAULT_ARGS` | `--dry-run` |
| `SCORE_FILE` | `tech-debt-score.txt` |
| `KEYBINDING` | `cmd+shift+t` |

### Quick Examples

#### VS Code Task
\```json
{
  "label": "Find Tech Debt",
  "type": "shell",
  "command": "sub-agent-tech-debt-finder-fixer --dry-run",
  "group": "build"
}
\```

#### Pre-commit Hook
\```bash
#!/bin/sh
sub-agent-tech-debt-finder-fixer --dry-run --quiet
if [ $? -ne 0 ]; then
    echo "❌ Tech debt issues found"
    echo "💡 Run 'sub-agent-tech-debt-finder-fixer --interactive' to fix"
    exit 1
fi
\```
```

### Example 2: Architecture Reviewer

**Current duplicated content in `sub-agent-architecture-reviewer.md`:**

```markdown
## Integration Examples

### VS Code Extension
\```json
{
  "command": "claude.architectureReview",
  "title": "Review Architecture",
  "key": "cmd+shift+a"
}
\```

### CI/CD Pipeline
\```yaml
architecture-check:
  script:
    - @architecture-review --detect-violations
    - if [ $? -ne 0 ]; then exit 1; fi
  artifacts:
    paths:
      - architecture-report.md
      - diagrams/
\```
```

**Migrated content:**

```markdown
## Integration Examples

For complete setup guides, see [shared integration examples](../shared/examples/):

- **[VS Code Integration](../shared/examples/vscode-integration.md#architecture-reviewer-integration)** - Extension commands and tasks
- **[CI/CD Pipelines](../shared/examples/cicd-integration.md#architecture-reviewer)** - Multi-platform pipeline configurations
- **[Git Hooks](../shared/examples/git-hooks-integration.md#architecture-review-pre-push)** - Pre-push validation hooks

### Command-Specific Parameters

| Parameter | Value |
|-----------|-------|
| `COMMAND_NAME` | `sub-agent-architecture-reviewer` |
| `COMMAND_ID` | `architectureReview` |
| `COMMAND_TITLE` | `Review Architecture` |
| `DEFAULT_ARGS` | `--detect-violations` |
| `REPORT_PATHS` | `architecture-report.md, diagrams/` |
| `KEYBINDING` | `cmd+shift+a` |

### Architecture-Specific Examples

#### Advanced VS Code Task with Problem Matcher
\```json
{
  "label": "Architecture Violations Check",
  "type": "shell", 
  "command": "sub-agent-architecture-reviewer --detect-violations",
  "group": "build",
  "problemMatcher": {
    "pattern": {
      "regexp": "^(.*):(\\\\d+):(\\\\d+):\\\\s+(warning|error):\\\\s+(.*)$",
      "file": 1,
      "line": 2,
      "column": 3,
      "severity": 4,
      "message": 5
    }
  }
}
\```
```

### Example 3: Refactor Command

**Current duplicated content in `sub-agent-refactor.md`:**

```markdown
## Integration Examples

### VSCode Task
\```json
{
  "label": "Refactor Current File",
  "type": "shell",
  "command": "sub-agent-refactor ${file} --interactive",
  "group": "build"
}
\```

### Git Pre-commit Hook
\```bash
#!/bin/sh
sub-agent-refactor src/ --dry-run --type auto
\```

### CI/CD Pipeline
\```yaml
- name: Code Quality Check
  run: |
    sub-agent-refactor src/ --dry-run
    if [ $? -ne 0 ]; then
      echo "Refactoring opportunities found"
      exit 1
    fi
\```
```

**Migrated content:**

```markdown
## Integration Examples

See [shared integration examples](../shared/examples/) for comprehensive setup:

- **[VS Code Integration](../shared/examples/vscode-integration.md#refactor-integration)** - Interactive and batch refactoring tasks
- **[CI/CD Pipelines](../shared/examples/cicd-integration.md#refactor)** - Quality check integrations
- **[Git Hooks](../shared/examples/git-hooks-integration.md#refactoring-opportunities-post-commit)** - Pre-commit and post-commit hooks

### Command-Specific Parameters

| Parameter | Value |
|-----------|-------|
| `COMMAND_NAME` | `sub-agent-refactor` |
| `COMMAND_ID` | `refactor` |
| `COMMAND_TITLE` | `Refactor Code` |
| `DEFAULT_ARGS` | `src/ --dry-run` |
| `INTERACTIVE_ARGS` | `${file} --interactive` |
| `EXIT_CONDITION` | `if [ $? -ne 0 ]; then echo "Refactoring opportunities found"; exit 1; fi` |
```

## Migration Checklist

For each command file being migrated:

### ✅ Analysis Phase
- [ ] Identify all integration sections in the file
- [ ] Compare with shared templates to find matches
- [ ] Note any unique examples that need to be preserved
- [ ] Determine appropriate parameters for the command

### ✅ Migration Phase
- [ ] Replace duplicated VS Code examples with references
- [ ] Replace duplicated CI/CD examples with references  
- [ ] Replace duplicated Git hook examples with references
- [ ] Add command-specific parameter table
- [ ] Preserve any unique integration examples
- [ ] Update section headings and organization

### ✅ Validation Phase
- [ ] Test VS Code integration examples work
- [ ] Test CI/CD pipeline examples work
- [ ] Test Git hook examples work
- [ ] Verify all links point to correct shared template sections
- [ ] Check that parameter values are correct
- [ ] Ensure no broken references

### ✅ Documentation Phase
- [ ] Update table of contents if needed
- [ ] Add links to shared examples in overview
- [ ] Remove outdated integration documentation
- [ ] Add migration notes if breaking changes

## Files Requiring Migration

### High Priority (Multiple Duplications)
1. **`sub-agent-tech-debt-finder-fixer.md`** - VS Code + CI/CD + Git hooks
2. **`sub-agent-architecture-reviewer.md`** - VS Code + CI/CD  
3. **`README.md`** - VS Code + CI/CD + Git hooks
4. **`sub-agent-refactor.md`** - VS Code + CI/CD + Git hooks

### Medium Priority (Some Duplications)
5. **`sub-agent-new-feature.md`** - CI/CD examples
6. **`SETUP.md`** - CI/CD examples
7. **`sub-agent-test-generator.md`** - Minimal integration content
8. **`sub-agent-performance-optimizer.md`** - Minimal integration content

### Low Priority (Minimal Duplications)
9. **`sub-agent-migration-assistant.md`** - Unique integration patterns
10. **`sub-agent-tech-debt-finder-parallel.md`** - Specialized examples

## Template Customization Guidelines

### When to Use Shared Templates As-Is
- Standard VS Code task configurations
- Basic CI/CD pipeline jobs
- Simple pre-commit hooks
- Common parameter patterns

### When to Extend Shared Templates
- Command has unique arguments or behaviors
- Integration requires special setup steps
- Error handling needs customization
- Multiple variants are needed for different use cases

### When to Create Command-Specific Examples
- Integration pattern is truly unique to the command
- Shared template doesn't cover the use case
- Command requires complex multi-step integration
- Platform-specific requirements exist

## Quality Assurance

### Before Migration
1. **Document current examples** - Screenshot or copy current integration sections
2. **Test current examples** - Verify they work as documented
3. **Identify dependencies** - Note any command-specific requirements

### During Migration
1. **Validate each replacement** - Ensure shared template covers the same functionality
2. **Check parameter accuracy** - Verify all parameters are correctly filled
3. **Test incrementally** - Validate each section as you migrate it

### After Migration
1. **Full integration testing** - Test all VS Code, CI/CD, and Git hook examples
2. **Link validation** - Ensure all references point to correct locations
3. **Parameter verification** - Double-check all parameter values are accurate
4. **Documentation review** - Verify clarity and completeness

## Rollback Plan

If migration causes issues:

1. **Immediate rollback** - Restore from git history
2. **Identify problem** - Determine what went wrong
3. **Fix shared template** - Update template to handle the use case
4. **Re-attempt migration** - Try again with improved template
5. **Document lessons learned** - Update migration guide with findings

## Success Metrics

Migration is successful when:
- ✅ **Reduction in duplication** - Integration examples appear only in shared templates
- ✅ **Maintained functionality** - All integration examples still work correctly
- ✅ **Improved maintainability** - Updates only need to be made in shared templates
- ✅ **Better discoverability** - Users can find all integration options in one place
- ✅ **Consistent quality** - All commands have the same high-quality integration examples

## Support

For migration assistance:
1. **Review this guide** thoroughly before starting
2. **Check shared templates** for similar command examples
3. **Test thoroughly** after each migration step
4. **Document unique cases** that don't fit shared templates
5. **Open issues** for template improvements needed