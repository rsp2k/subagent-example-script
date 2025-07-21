# Claude Sub-Agent Commands

This directory contains the actual command prompt files that can be installed in Claude Code's commands directory (`~/.claude/commands/`).

## Command Categories

### Core Commands (Original Sequential)
- **[sub-agent-tech-debt-finder-fixer.md](./sub-agent-tech-debt-finder-fixer.md)** - Two-phase tech debt identification and fixing
- **[sub-agent-architecture-reviewer.md](./sub-agent-architecture-reviewer.md)** - System architecture analysis and visualization
- **[sub-agent-test-generator.md](./sub-agent-test-generator.md)** - Comprehensive test suite generation
- **[sub-agent-performance-optimizer.md](./sub-agent-performance-optimizer.md)** - Full-stack performance optimization
- **[sub-agent-migration-assistant.md](./sub-agent-migration-assistant.md)** - Framework and library migration assistance
- **[sub-agent-refactor.md](./sub-agent-refactor.md)** - Intelligent code refactoring with verification
- **[sub-agent-new-feature.md](./sub-agent-new-feature.md)** - New feature development using existing patterns

### Enhanced Parallel Commands
- **[sub-agent-tech-debt-finder-parallel.md](./sub-agent-tech-debt-finder-parallel.md)** - True parallel execution demonstration

## Installation

### Quick Install All Commands
```bash
# Install all commands
cp commands/*.md ~/.claude/commands/

# Or install selectively
cp commands/sub-agent-tech-debt-finder-parallel.md ~/.claude/commands/
```

### Verify Installation
```bash
ls ~/.claude/commands/sub-agent-*
```

## Usage

Once installed, commands can be used with their aliases:

```bash
# Original commands
@tech-debt-finder-fixer src/
@arch-review . --detect-violations
@test-gen --only-untested
@perf-optimize --bundle-analysis
@migrate --from react@17 --to react@18
@refactor src/components/ --type extract-component
@new-feature --feature "user dashboard" --location src/pages/

# Enhanced parallel command
@tech-debt-parallel . --dry-run
```

## Command Evolution

### Sequential → Parallel Enhancement

The repository demonstrates the evolution from sequential sub-agent simulation to true parallel execution:

**Original Pattern (Sequential):**
```
Agent 1 → Agent 2 → Agent 3 → Synthesis → Implementation
```

**Enhanced Pattern (Parallel):**
```
Agent 1 ↘
Agent 2 → Synthesis → Implementation Agents (parallel)
Agent 3 ↗
```

### Performance Comparison

| Approach | Small Codebase | Large Codebase | Improvement |
|----------|----------------|----------------|-------------|
| Sequential | 15 minutes | 240 minutes | Baseline |
| Parallel | 5 minutes | 80 minutes | **67% faster** |

## Development Guidelines

### Creating New Commands

When developing new sub-agent commands:

1. **Follow the 4-phase pattern**: Analysis → Synthesis → Planning → Implementation
2. **Use proper aliases**: Full, short, and acronym versions
3. **Include safety mechanisms**: Dry-run, interactive, backup modes
4. **Support configuration**: Project-specific config files
5. **Consider parallel enhancement**: Can agents work concurrently?

### Command Structure Template

```markdown
# Sub-Agent [Command Name]

## Command Syntax
command-name [target] [options]

## Aliases
@full-name [target] [options]
@short-name [target] [options]
@acronym [target] [options]

## Phase 1: Analysis Agents (Parallel)
[Agent specifications...]

## Phase 2: Synthesis
[Synthesis agent specification...]

## Phase 3: Implementation
[Implementation approach...]

## Configuration
[Config file support...]

## Examples
[Usage examples...]
```

## Best Practices

### Command Design
- **Single responsibility**: Each command should have one clear purpose
- **Composable**: Commands should work well together
- **Safe by default**: Include dry-run and backup options
- **User-friendly**: Provide helpful error messages and guidance

### Agent Design
- **Specialized expertise**: Each agent should have focused domain knowledge
- **Independent execution**: Agents should not depend on each other's results
- **Structured output**: Consistent format for synthesis layer
- **Error resilience**: Graceful handling of partial failures

### Documentation
- **Clear examples**: Show real-world usage scenarios
- **Parameter reference**: Document all options and flags
- **Integration guides**: Show how to use with IDEs and CI/CD
- **Troubleshooting**: Common issues and solutions

## Future Enhancements

### Planned Parallel Commands
- **sub-agent-architecture-reviewer-parallel.md** - Concurrent architecture analysis
- **sub-agent-performance-optimizer-parallel.md** - Parallel performance optimization
- **sub-agent-test-generator-parallel.md** - Concurrent test generation

### Advanced Patterns
- **Hierarchical parallelism**: Multi-level concurrent execution
- **Adaptive agent selection**: Dynamic agent combinations based on codebase
- **Cross-command coordination**: Commands that work together on complex workflows

## Contributing

### Adding New Commands
1. Create command file following the template
2. Test thoroughly with various codebases
3. Add documentation and examples
4. Update this README with command description

### Enhancing Existing Commands
1. Maintain backward compatibility
2. Add parallel execution where beneficial
3. Improve agent specialization
4. Enhance error handling and user experience

### Reporting Issues
- Test commands in isolation first
- Provide example codebase that demonstrates the issue
- Include command output and error messages
- Suggest improvements or alternative approaches

---

*For usage documentation, see [../docs/README.md](../docs/README.md). For implementation guidance, see [../PARALLEL-IMPLEMENTATION-GUIDE.md](../PARALLEL-IMPLEMENTATION-GUIDE.md).*