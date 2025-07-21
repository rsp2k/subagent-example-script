# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This repository contains a collection of Claude sub-agent command examples that demonstrate multi-agent orchestration patterns for complex software engineering tasks. It provides 7 powerful commands that can be installed as Claude Code commands to coordinate multiple specialized AI agents.

## Architecture

### Command Structure
The repository consists of individual command files written as markdown documents that contain detailed prompts for orchestrating multiple sub-agents:

- **sub-agent-tech-debt-finder-fixer.md** - Two-phase tech debt identification and fixing
- **sub-agent-architecture-reviewer.md** - System architecture analysis and visualization  
- **sub-agent-test-generator.md** - Comprehensive test suite generation
- **sub-agent-performance-optimizer.md** - Full-stack performance optimization
- **sub-agent-migration-assistant.md** - Framework and library migration assistance
- **sub-agent-refactor.md** - Intelligent code refactoring with verification
- **sub-agent-new-feature.md** - New feature development using existing patterns
- **sub-agent-tech-debt-finder-parallel.md** - Enhanced version demonstrating true parallel execution

### Multi-Agent Orchestration Pattern
Each command follows a consistent 4-phase orchestration pattern:

1. **Analysis Phase** - Multiple specialized agents analyze different aspects in parallel
2. **Synthesis Phase** - Results are combined into unified insights  
3. **Planning Phase** - Actionable plans are created with priorities and dependencies
4. **Implementation Phase** - Specialized agents execute changes with verification

### True Parallel Execution Enhancement
The repository now includes enhanced versions that demonstrate how to leverage Claude Code's actual `Task` tool for true parallel execution rather than sequential "sub-agent" simulation. The parallel approach provides:

- **Real Parallelism**: Multiple agents execute simultaneously using concurrent Task calls
- **Faster Analysis**: 3-5x speed improvement in analysis phases
- **Better Resource Utilization**: Leverages Claude Code's concurrent capabilities
- **Scalable Architecture**: Can handle large codebases efficiently
- **Production-Ready Patterns**: Demonstrates sophisticated multi-agent workflows

### Command Installation
Commands are designed to be installed in the Claude Code commands directory (`~/.claude/commands/`) and used with aliases like `@tech-debt-finder-fixer` or `@refactor`.

## Key Features

### Sub-Agent Specialization
- **Analysis Agents**: Duplicate detection, complexity analysis, pattern recognition, dead code identification
- **Planning Agents**: Task prioritization, dependency mapping, risk assessment
- **Implementation Agents**: Code extraction, file splitting, pattern standardization
- **Verification Agents**: Test execution, behavior validation, rollback capabilities

### Safety Mechanisms
- Dry-run modes for previewing changes
- Interactive review of each modification
- Automatic backup creation before changes
- Test execution between modifications
- Rollback capabilities for failed changes

### Configuration Support
Commands support project-specific configuration files:
- `.techdebtignore` - Patterns to ignore during analysis
- `.architecture-rules.yaml` - Custom architecture validation rules
- `.performance.config.js` - Performance targets and thresholds

## Development Guidelines

### Command Development
When working with or extending these commands:

1. **Follow the 4-phase pattern** for new sub-agent orchestrations
2. **Include safety mechanisms** like dry-run and interactive modes
3. **Provide multiple aliases** for ease of use
4. **Support configuration files** for project customization
5. **Include verification steps** after each modification

### Testing Commands
Since there are no package.json scripts, test commands by:
1. Installing them in `~/.claude/commands/`
2. Using them in test projects with `--dry-run` first
3. Verifying the multi-agent orchestration works as expected

### Implementing True Parallel Execution
When enhancing commands with parallel execution using the Task tool:
1. **Launch concurrent Tasks**: Use multiple Task calls in a single message for parallel analysis
2. **Structure agent outputs**: Each Task should return structured data for synthesis
3. **Handle coordination**: Design synthesis agents to correlate findings across parallel agents
4. **Maintain safety**: Preserve dry-run and interactive modes in parallel workflows
5. **Test scalability**: Ensure parallel patterns work with large codebases

### Documentation
Each command file contains:
- Complete command syntax and options
- Detailed examples for common scenarios
- Explanation of the sub-agent orchestration flow
- Configuration options and customization

## Common Usage Patterns

### Development Workflow
```bash
# Clean up tech debt
@tech-debt-finder-fixer src/ --dry-run

# Generate missing tests  
@test-gen --only-untested --coverage-target 80

# Review architecture
@arch-review --detect-violations

# Optimize performance
@perf-optimize --bundle-analysis
```

### Migration Workflow
```bash
# Review current state
@arch-review --suggest-improvements

# Clean up before migration
@tech-debt-finder-fixer --fix-order quick-wins

# Execute migration
@migrate --from react@17 --to react@18 --incremental
```

### Feature Development
```bash
# Build new feature using existing patterns
@new-feature --feature "user dashboard" --location src/pages/

# Refactor if needed
@refactor src/components/ --type extract-component

# Add comprehensive tests
@test-gen src/pages/dashboard/ --type all
```

## File Structure

- **README.md** - Main documentation and feature overview
- **SETUP.md** - Installation and usage instructions
- **sub-agent-*.md** - Individual command implementations
- Each command file is self-contained with full orchestration logic

This repository serves as both a collection of useful development commands and a reference implementation for building sophisticated multi-agent AI systems for software engineering tasks.