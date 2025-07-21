# Command Syntax Reference

Complete syntax reference for all parallel sub-agent commands. Use this for quick lookup while working.

## Universal Syntax Pattern

All commands follow this consistent pattern:

```bash
command-name [target] [options]

# With aliases
@alias [target] [options]
```

**Components:**
- `command-name` - Full command name or alias
- `target` - Directory, file, or glob pattern (optional, defaults to current directory)
- `options` - Command-specific flags and parameters

## Core Commands

### Tech Debt Finder & Fixer

```bash
sub-agent-tech-debt-finder-fixer [target] [options]

# Aliases
@tech-debt-finder-fixer [target] [options]
@tech-debt [target] [options]
@satdff [target] [options]
```

### Architecture Reviewer

```bash
sub-agent-architecture-reviewer [target] [options]

# Aliases
@architecture-review [target] [options]
@arch-review [target] [options]
@saar [target] [options]
```

### Test Generator

```bash
sub-agent-test-generator [target] [options]

# Aliases
@test-generator [target] [options]
@test-gen [target] [options]
@satg [target] [options]
```

### Performance Optimizer

```bash
sub-agent-performance-optimizer [target] [options]

# Aliases
@performance-optimizer [target] [options]
@perf-optimize [target] [options]
@sapo [target] [options]
```

### Migration Assistant

```bash
sub-agent-migration-assistant [target] [options]

# Aliases
@migration-assistant [target] [options]
@migrate [target] [options]
@sama [target] [options]
```

### Refactor Command

```bash
sub-agent-refactor [target] [options]

# Aliases
@refactor [target] [options]
@refact [target] [options]
@sar [target] [options]
```

### New Feature Builder

```bash
sub-agent-new-feature [target] [options]

# Aliases
@new-feature [target] [options]
@feature [target] [options]
@sanf [target] [options]
```

## Enhanced Parallel Commands

### Parallel Tech Debt Finder

```bash
sub-agent-tech-debt-finder-parallel [target] [options]

# Aliases
@tech-debt-parallel [target] [options]
@tdp [target] [options]
```

## Target Specifications

### Directory Targets
```bash
# Current directory
@command

# Specific directory
@command src/

# Multiple directories
@command src/ tests/ docs/

# Relative paths
@command ../other-project/src/

# Absolute paths
@command /home/user/project/src/
```

### File Targets
```bash
# Single file
@command src/main.js

# Multiple files
@command src/main.js src/utils.js

# Glob patterns
@command "src/**/*.js"
@command "**/*.{ts,tsx}"
@command "src/components/*.jsx"
```

### Pattern Examples
```bash
# JavaScript/TypeScript files
@command "**/*.{js,ts,jsx,tsx}"

# Configuration files
@command "*.{json,yaml,yml,toml}"

# Documentation files
@command "**/*.md"

# Exclude patterns (use with --exclude)
@command src/ --exclude "**/*.test.js"
```

## Common Options

### Execution Control
```bash
--dry-run              # Preview changes without applying
--interactive          # Review each change before applying
--auto-fix <level>     # Automatic fixing level: none|safe|recommended|all
--max-changes <number> # Maximum number of changes per run
--timeout <ms>         # Operation timeout in milliseconds
```

### Output Control
```bash
--output <format>      # Output format: text|json|markdown|html
--verbose              # Detailed output with progress information
--quiet                # Minimal output, errors only
--no-color             # Disable colored output
--log-level <level>    # Logging level: debug|info|warn|error
```

### Safety Options
```bash
--backup               # Create backup files before changes (default: true)
--no-backup            # Skip backup creation
--test-command <cmd>   # Custom test command to run after changes
--rollback-on-fail     # Automatically rollback if tests fail
--safe-mode            # Extra conservative change detection
```

### Filtering Options
```bash
--include <pattern>    # Include files matching pattern
--exclude <pattern>    # Exclude files matching pattern
--type <types>         # Specific analysis types (comma-separated)
--threshold <level>    # Sensitivity: conservative|standard|aggressive
--min-severity <level> # Minimum issue severity: low|medium|high|critical
```

## Command-Specific Options

### Tech Debt Finder Options
```bash
--type <types>         # Types: duplicates|complexity|patterns|dead-code|types|all
--fix-order <order>    # Order: risk|impact|quick-wins|custom
--similarity <float>   # Duplicate detection threshold (0.0-1.0)
--complexity-limit <n> # Maximum cyclomatic complexity
--file-size-limit <n>  # Maximum lines per file
```

### Architecture Reviewer Options
```bash
--detect-violations    # Find architecture rule violations
--suggest-improvements # Generate improvement suggestions
--style <style>        # Architecture style: mvc|hexagonal|ddd|microservices
--visualize            # Generate architecture diagrams
--output-format <fmt>  # Diagram format: mermaid|plantuml|graphviz
--depth <level>        # Analysis depth: shallow|standard|deep
```

### Test Generator Options
```bash
--only-untested        # Generate tests only for untested code
--coverage-target <n>  # Target coverage percentage (0-100)
--test-types <types>   # Types: unit|integration|e2e|all
--framework <name>     # Test framework: jest|mocha|pytest|phpunit
--mock-strategy <type> # Mocking: minimal|realistic|comprehensive
--update-existing      # Update existing test files
```

### Performance Optimizer Options
```bash
--target <targets>     # Targets: frontend|backend|database|all
--bundle-analysis      # Analyze bundle size and composition
--suggest-indexes      # Suggest database indexes
--cache-strategy       # Develop caching recommendations
--baseline <file>      # Performance baseline file
--threshold <metrics>  # Performance thresholds
```

### Migration Assistant Options
```bash
--from <version>       # Source version (e.g., react@17)
--to <version>         # Target version (e.g., react@18)
--pattern <pattern>    # Migration pattern: class-to-hooks|api-v2|etc
--incremental          # Gradual migration approach
--compatibility-check # Check compatibility before migration
--rollback-plan        # Generate rollback procedures
```

### Refactor Options
```bash
--type <types>         # Types: extract-method|extract-component|reduce-complexity|split-file
--max-function-size <n> # Maximum lines per function
--max-file-size <n>    # Maximum lines per file
--max-complexity <n>   # Maximum cyclomatic complexity
--create-shared        # Create shared utilities from duplicates
--preserve-structure   # Maintain current file structure
```

### New Feature Options
```bash
--feature <description> # Feature description in quotes
--location <path>       # Target location for new feature
--type <type>           # Feature type: component|service|page|api|full-stack
--tech-stack <stack>    # Technology stack: react|vue|laravel|django
--example-files <files> # Example files to learn patterns from
--style-guide <guide>   # Style guide to follow
--test-first           # Generate tests before implementation
```

## Configuration File Options

### Global Configuration
```bash
--config <file>        # Use specific configuration file
--no-config            # Ignore configuration files
--config-path <path>   # Look for config in specific directory
```

### Configuration File Formats

**JavaScript (.js):**
```javascript
module.exports = {
  includePaths: ['src/**/*.js'],
  excludePaths: ['node_modules/**'],
  thresholds: { maxFileLines: 300 }
};
```

**JSON (.json):**
```json
{
  "includePaths": ["src/**/*.js"],
  "excludePaths": ["node_modules/**"],
  "thresholds": { "maxFileLines": 300 }
}
```

**YAML (.yaml/.yml):**
```yaml
includePaths:
  - "src/**/*.js"
excludePaths:
  - "node_modules/**"
thresholds:
  maxFileLines: 300
```

## Environment Variables

### Global Settings
```bash
CLAUDE_TASK_TIMEOUT=300000     # Default timeout in milliseconds
CLAUDE_MAX_CONCURRENT=5        # Maximum concurrent agents
CLAUDE_OUTPUT_FORMAT=json      # Default output format
CLAUDE_LOG_LEVEL=info          # Default logging level
CLAUDE_CONFIG_PATH=.           # Default config file location
```

### Command-Specific Variables
```bash
CLAUDE_BACKUP_DIR=.backups     # Backup directory location
CLAUDE_TEST_COMMAND="npm test" # Default test command
CLAUDE_EDITOR=code             # Default editor for interactive mode
CLAUDE_DIFF_TOOL=diff          # Tool for showing differences
```

## Exit Codes

```bash
0   # Success
1   # General error
2   # Invalid arguments
3   # Configuration error
4   # Target not found
5   # Permission denied
6   # Analysis failed
7   # Implementation failed
8   # Tests failed
9   # Timeout
10  # User cancelled
```

## Examples

### Basic Usage
```bash
# Analyze current directory with defaults
@tech-debt

# Analyze specific directory
@arch-review src/

# Generate tests for untested code
@test-gen --only-untested --coverage-target 80
```

### Advanced Usage
```bash
# Complex tech debt analysis
@tech-debt src/ --type duplicates,complexity --threshold aggressive --auto-fix safe --max-changes 10

# Architecture review with visualization
@arch-review . --detect-violations --visualize --output-format mermaid --depth deep

# Migration with rollback plan
@migrate --from react@17 --to react@18 --incremental --rollback-plan
```

### Batch Processing
```bash
# Process multiple directories
for dir in frontend backend shared; do
  @tech-debt "$dir/" --output "$dir-debt.md"
done

# Chain commands
@arch-review . --dry-run && @tech-debt . --auto-fix safe && @test-gen --only-untested
```

### Integration Examples
```bash
# CI/CD usage
@tech-debt . --dry-run --output json | tee tech-debt-report.json

# With custom test command
@refactor src/ --test-command "npm run test:unit && npm run lint"

# With specific configuration
@perf-optimize . --config .performance.config.js --baseline perf-baseline.json
```

---

*For detailed parameter explanations, see [Parameter Reference](./parameters.md).*