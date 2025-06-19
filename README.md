# Claude Sub-Agent Command Examples

A collection of powerful sub-agent orchestration commands for Claude that demonstrate how to coordinate multiple specialized AI agents to tackle complex software engineering tasks.

## Overview

These commands showcase the power of multi-agent systems where specialized agents work in parallel to analyze, plan, and execute complex tasks. Each command orchestrates multiple sub-agents with specific expertise to deliver comprehensive solutions.

## Available Commands

### 1. [Tech Debt Finder & Fixer](./sub-agent-tech-debt-finder-fixer.md)
**Aliases:** `@tech-debt-finder-fixer`, `@td-finder-fixer`, `@satdff`

A two-phase command that identifies and fixes technical debt:
- **Phase 1:** 5 parallel agents analyze code for duplicates, complexity, patterns, dead code, and type coverage
- **Phase 2:** Specialized fix agents safely refactor code with automated testing

**Key Features:**
- Finds duplicate code (70%+ similarity)
- Identifies oversized files and complex functions
- Detects inconsistent patterns
- Removes dead code and unused dependencies
- Improves TypeScript type coverage
- Creates prioritized fix plan with time estimates

### 2. [Architecture Reviewer](./sub-agent-architecture-reviewer.md)
**Aliases:** `@architecture-review`, `@arch-review`, `@saar`

Analyzes system architecture and generates visual documentation:
- Dependency mapping and circular dependency detection
- Pattern analysis (MVC, Hexagonal, DDD compliance)
- Complexity metrics and cognitive load assessment
- Generates Mermaid/PlantUML diagrams

**Key Features:**
- Detects layer violations and anti-patterns
- Creates component and sequence diagrams
- Provides improvement roadmap
- Supports custom architecture rules
- Calculates maintainability metrics

### 3. [Test Generator](./sub-agent-test-generator.md)
**Aliases:** `@test-gen`, `@test-generator`, `@satg`

Generates comprehensive test suites by analyzing code:
- Coverage analysis to find untested paths
- Code path analysis for behavior mapping
- Pattern recognition for consistent test style
- Parallel generation of unit, integration, and E2E tests

**Key Features:**
- Achieves target coverage (default 80%)
- Generates meaningful test cases with edge cases
- Creates appropriate mocks and fixtures
- Follows existing test patterns
- Optimizes generated tests for maintainability

### 4. [Performance Optimizer](./sub-agent-performance-optimizer.md)
**Aliases:** `@perf-optimize`, `@performance`, `@sapo`

Full-stack performance optimization across all layers:
- Frontend bundle analysis and React optimization
- Backend API and query optimization
- Database index and schema optimization
- Caching strategy development

**Key Features:**
- Bundle size reduction through tree shaking
- React re-render optimization
- Database query optimization with index suggestions
- Caching strategy implementation
- Performance metric tracking and reporting

### 5. [Migration Assistant](./sub-agent-migration-assistant.md)
**Aliases:** `@migrate`, `@migration-assistant`, `@sama`

Assists with framework and library migrations:
- Deprecation scanning and compatibility analysis
- Pattern detection and migration planning
- Automated codemod execution
- Incremental migration with rollback support

**Supported Migrations:**
- React 16→17→18, Class→Hooks
- Laravel 8→9→10
- JavaScript→TypeScript
- Database version upgrades
- And many more...

## How Sub-Agent Orchestration Works

### 1. Parallel Analysis Phase
Multiple specialized agents analyze different aspects simultaneously:
```
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│   Agent 1   │  │   Agent 2   │  │   Agent 3   │
│ (Duplicates)│  │(Complexity) │  │ (Patterns)  │
└─────────────┘  └─────────────┘  └─────────────┘
       │                 │                 │
       └─────────────────┴─────────────────┘
                         │
                  ┌──────────────┐
                  │  Synthesis   │
                  │    Agent     │
                  └──────────────┘
```

### 2. Planning Phase
A synthesis agent combines findings and creates an actionable plan:
- Prioritizes issues by impact and effort
- Groups related tasks
- Identifies dependencies
- Estimates time and risk

### 3. Implementation Phase
Specialized fix agents work in parallel on approved changes:
```
┌─────────────┐  ┌─────────────┐  ┌─────────────┐
│  Extractor  │  │   Splitter  │  │ Standardizer│
│    Agent    │  │    Agent    │  │    Agent    │
└─────────────┘  └─────────────┘  └─────────────┘
```

### 4. Verification Phase
A verification agent ensures all changes are successful:
- Runs tests after each change
- Validates behavior preservation
- Checks performance impact
- Provides rollback if needed

## Command Syntax Pattern

All commands follow a similar pattern:
```bash
command-name [target] [options]

# With aliases
@alias [target] [options]
```

Common options:
- `--dry-run` - Preview changes without applying
- `--interactive` - Review each change
- `--auto-fix` - Apply safe fixes automatically
- `--test-command` - Custom test command
- `--backup` - Create backups (usually default)

## Best Practices

1. **Start with Analysis**: Run commands in dry-run mode first
2. **Use Interactive Mode**: Review changes before applying
3. **Have Good Tests**: Ensure test coverage before major refactoring
4. **Work Incrementally**: Fix issues in small batches
5. **Monitor Progress**: Track improvements over time
6. **Customize Rules**: Define project-specific standards

## Configuration

Most commands support configuration files:

```javascript
// .techdebt.config.js
module.exports = {
  thresholds: {
    maxFileLines: 300,
    maxFunctionLines: 30,
    maxComplexity: 10
  },
  ignore: ['vendor/', 'legacy/']
};
```

## Integration

### VS Code
Add to tasks.json:
```json
{
  "label": "Find Tech Debt",
  "type": "shell",
  "command": "@tech-debt-finder-fixer --dry-run"
}
```

### CI/CD Pipeline
```yaml
code-quality:
  script:
    - @architecture-review --detect-violations
    - @tech-debt-finder-fixer --dry-run
```

### Git Hooks
```bash
#!/bin/sh
# pre-push hook
@test-gen --only-untested --coverage-target 80
```

## Benefits of Multi-Agent Approach

1. **Parallel Processing**: Multiple agents work simultaneously
2. **Specialized Expertise**: Each agent focuses on its domain
3. **Comprehensive Analysis**: No aspect is overlooked
4. **Intelligent Coordination**: Agents share findings for better decisions
5. **Safe Implementation**: Changes are verified at each step
6. **Scalable**: Can handle large codebases efficiently

## Contributing

These commands demonstrate patterns for creating your own multi-agent commands. Key principles:

1. **Clear Agent Roles**: Each agent should have a specific purpose
2. **Structured Communication**: Define clear interfaces between agents
3. **Progressive Enhancement**: Build complex behaviors from simple agents
4. **Safety First**: Always include verification and rollback
5. **User Control**: Provide dry-run and interactive modes

## License

These examples are provided as reference implementations for Claude sub-agent orchestration patterns.