# VS Code Integration Templates

This file contains reusable VS Code integration examples that can be parameterized for different sub-agent commands.

## Command Definition Template

```json
{
  "command": "claude.{{COMMAND_ID}}",
  "title": "{{COMMAND_TITLE}}",
  "key": "{{KEYBINDING}}"
}
```

## Task Definition Template

```json
{
  "label": "{{TASK_LABEL}}",
  "type": "shell",
  "command": "{{COMMAND_NAME}} {{DEFAULT_ARGS}}",
  "group": "{{GROUP}}",
  "presentation": {
    "echo": true,
    "reveal": "always",
    "focus": false,
    "panel": "shared"
  },
  "problemMatcher": []
}
```

## Interactive Task Template

```json
{
  "label": "{{TASK_LABEL}} (Interactive)",
  "type": "shell",
  "command": "{{COMMAND_NAME}} ${file} --interactive",
  "group": "{{GROUP}}",
  "presentation": {
    "echo": true,
    "reveal": "always",
    "focus": true,
    "panel": "new"
  }
}
```

## Common Parameter Values

### Command IDs
- `techDebtFinder` - Tech Debt Finder & Fixer
- `architectureReview` - Architecture Reviewer
- `testGenerator` - Test Generator
- `refactor` - Code Refactorer
- `newFeature` - New Feature Builder
- `performanceOptimizer` - Performance Optimizer
- `migrationAssistant` - Migration Assistant

### Task Groups
- `build` - Build-related tasks
- `test` - Testing tasks
- `none` - No specific group

### Common Keybindings
- `cmd+shift+a` - Architecture Review
- `cmd+shift+t` - Tech Debt Finder
- `cmd+shift+g` - Test Generator
- `cmd+shift+r` - Refactor
- `cmd+shift+n` - New Feature
- `cmd+shift+p` - Performance Optimizer

## Usage Examples

### Tech Debt Finder Integration
```json
{
  "command": "claude.techDebtFinder",
  "title": "Find and Fix Tech Debt",
  "key": "cmd+shift+t"
}
```

### Architecture Reviewer Integration
```json
{
  "command": "claude.architectureReview",
  "title": "Review Architecture",
  "key": "cmd+shift+a"
}
```

### Quick Refactor Task
```json
{
  "label": "Refactor Current File",
  "type": "shell",
  "command": "sub-agent-refactor ${file} --interactive",
  "group": "build"
}
```

### Generate Tests Task
```json
{
  "label": "Generate Tests",
  "type": "shell",
  "command": "sub-agent-test-generator ${fileDirname} --only-untested",
  "group": "test"
}
```

## Advanced Configurations

### Multi-step Task with Input
```json
{
  "label": "New Feature with Location",
  "type": "shell",
  "command": "sub-agent-new-feature --feature '${input:featureName}' --location '${input:featureLocation}'",
  "group": "build"
}
```

### Task with Problem Matcher
```json
{
  "label": "Architecture Violations Check",
  "type": "shell",
  "command": "sub-agent-architecture-reviewer --detect-violations",
  "group": "build",
  "problemMatcher": {
    "pattern": {
      "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
      "file": 1,
      "line": 2,
      "column": 3,
      "severity": 4,
      "message": 5
    }
  }
}
```