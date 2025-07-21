# Parameter Reference

Complete reference for all command parameters and options. Use this for quick lookup of parameter meanings, valid values, and usage patterns.

## Universal Parameters

These parameters are available across all commands:

### Execution Control

#### `--dry-run`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Preview changes without applying them

```bash
@tech-debt . --dry-run
```

**Behavior:**
- Analyzes code and shows what would be changed
- Creates no files, makes no modifications
- Useful for understanding impact before committing
- Always safe to run

#### `--interactive`
**Type:** Boolean  
**Default:** `true`  
**Purpose:** Review each change before applying

```bash
@refactor . --interactive
```

**Behavior:**
- Pauses before each modification for user approval
- Shows diff preview for each change
- Allows skipping individual changes
- Can be disabled with `--no-interactive`

#### `--auto-fix`
**Type:** Enum  
**Values:** `none`, `safe`, `recommended`, `all`  
**Default:** `none`  
**Purpose:** Automatic application of changes

```bash
@tech-debt . --auto-fix safe
```

**Levels:**
- `none`: No automatic changes (review all)
- `safe`: Apply changes with minimal risk (imports, formatting)
- `recommended`: Apply moderate-risk changes (extract utilities, remove dead code)
- `all`: Apply all suggested changes (use with caution)

#### `--max-changes`
**Type:** Integer  
**Default:** `50`  
**Purpose:** Limit number of changes per run

```bash
@tech-debt . --max-changes 10
```

**Usage:**
- Prevents overwhelming number of changes
- Useful for gradual improvement approach
- Set to `0` for analysis-only mode
- Higher numbers for batch processing

#### `--timeout`
**Type:** Integer (milliseconds)  
**Default:** `300000` (5 minutes)  
**Purpose:** Operation timeout

```bash
@arch-review . --timeout 600000
```

**Guidelines:**
- Increase for large codebases
- Decrease for quick checks
- Maximum recommended: 600000 (10 minutes)
- Will gracefully cancel if exceeded

### Output Control

#### `--output`
**Type:** String (filename) or Enum (format)  
**Values:** `text`, `json`, `markdown`, `html`, or filename  
**Default:** `text`  
**Purpose:** Control output format and destination

```bash
@tech-debt . --output json
@tech-debt . --output tech-debt-report.md
```

**Formats:**
- `text`: Human-readable console output
- `json`: Structured data for programmatic use
- `markdown`: Documentation-friendly format
- `html`: Rich formatting with styling
- `filename`: Write to specified file

#### `--verbose`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Detailed output with progress information

```bash
@arch-review . --verbose
```

**Includes:**
- Agent execution progress
- Detailed analysis steps
- Performance timing information
- Intermediate results

#### `--quiet`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Minimal output, errors only

```bash
@tech-debt . --quiet
```

**Behavior:**
- Suppresses informational messages
- Shows only warnings and errors
- Useful for scripting and automation
- Cannot be combined with `--verbose`

#### `--no-color`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Disable colored output

```bash
@tech-debt . --no-color
```

**When to use:**
- CI/CD environments
- Log files
- Terminals without color support
- Accessibility requirements

#### `--log-level`
**Type:** Enum  
**Values:** `debug`, `info`, `warn`, `error`  
**Default:** `info`  
**Purpose:** Control logging verbosity

```bash
@arch-review . --log-level debug
```

**Levels:**
- `debug`: Everything including internal operations
- `info`: General information and progress
- `warn`: Warnings and potential issues
- `error`: Errors only

### Safety Options

#### `--backup`
**Type:** Boolean  
**Default:** `true`  
**Purpose:** Create backup files before changes

```bash
@refactor . --backup
```

**Behavior:**
- Creates `.bak` files for modified files
- Stored in `.backups/` directory by default
- Essential for safe refactoring
- Can be disabled with `--no-backup` (not recommended)

#### `--test-command`
**Type:** String  
**Default:** Auto-detected  
**Purpose:** Custom test command to run after changes

```bash
@refactor . --test-command "npm test && npm run lint"
```

**Auto-detection order:**
1. `npm test` (if package.json exists)
2. `composer test` (if composer.json exists)
3. `python -m pytest` (if pytest configured)
4. `cargo test` (if Cargo.toml exists)

#### `--rollback-on-fail`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Automatically rollback if tests fail

```bash
@refactor . --rollback-on-fail
```

**Behavior:**
- Runs tests after each change
- Reverts change if tests fail
- Continues with next change
- Requires `--test-command` or auto-detection

#### `--safe-mode`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Extra conservative change detection

```bash
@tech-debt . --safe-mode
```

**Effects:**
- More conservative similarity thresholds
- Additional validation steps
- Stricter safety checks
- Reduced false positives

### Filtering Options

#### `--include`
**Type:** Glob pattern(s)  
**Default:** All files  
**Purpose:** Include files matching pattern

```bash
@tech-debt . --include "src/**/*.{js,ts}"
@tech-debt . --include "*.py" --include "**/*.py"
```

**Pattern examples:**
- `"*.js"`: All JavaScript files in current directory
- `"**/*.ts"`: All TypeScript files recursively
- `"src/{components,utils}/**"`: Specific subdirectories

#### `--exclude`
**Type:** Glob pattern(s)  
**Default:** Common ignore patterns  
**Purpose:** Exclude files matching pattern

```bash
@tech-debt . --exclude "node_modules/**" --exclude "**/*.test.js"
```

**Default exclusions:**
- `node_modules/**`
- `.git/**`
- `dist/**`, `build/**`
- `*.min.js`, `*.bundle.js`
- `coverage/**`

#### `--type`
**Type:** Comma-separated list  
**Purpose:** Specific analysis types

```bash
@tech-debt . --type duplicates,complexity
@arch-review . --type violations,patterns
```

**Available types vary by command** (see command-specific sections below)

#### `--threshold`
**Type:** Enum  
**Values:** `conservative`, `standard`, `aggressive`  
**Default:** `standard`  
**Purpose:** Sensitivity level

```bash
@tech-debt . --threshold aggressive
```

**Levels:**
- `conservative`: High confidence, fewer false positives
- `standard`: Balanced approach
- `aggressive`: Lower confidence, catches more issues

#### `--min-severity`
**Type:** Enum  
**Values:** `low`, `medium`, `high`, `critical`  
**Default:** `low`  
**Purpose:** Minimum issue severity

```bash
@tech-debt . --min-severity high
```

**Usage:**
- Focus on most important issues first
- Filter out minor concerns
- Useful for initial cleanup phases

## Command-Specific Parameters

### Tech Debt Finder Parameters

#### `--type`
**Tech Debt Types:**
- `duplicates`: Duplicate code detection
- `complexity`: Code complexity analysis
- `patterns`: Pattern inconsistency detection
- `dead-code`: Unused code identification
- `types`: Type coverage analysis (TypeScript)
- `all`: All analysis types

#### `--fix-order`
**Type:** Enum  
**Values:** `risk`, `impact`, `quick-wins`, `custom`  
**Default:** `impact`

```bash
@tech-debt . --fix-order quick-wins
```

**Orders:**
- `risk`: Lowest risk changes first
- `impact`: Highest impact changes first
- `quick-wins`: Best effort/impact ratio first
- `custom`: User-defined order

#### `--similarity`
**Type:** Float (0.0-1.0)  
**Default:** `0.7`  
**Purpose:** Duplicate detection threshold

```bash
@tech-debt . --similarity 0.8
```

**Guidelines:**
- `0.9-1.0`: Only near-identical code
- `0.7-0.9`: Semantic duplicates (recommended)
- `0.5-0.7`: Structural similarities
- `<0.5`: May produce false positives

#### `--complexity-limit`
**Type:** Integer  
**Default:** `10`  
**Purpose:** Maximum cyclomatic complexity

```bash
@tech-debt . --complexity-limit 8
```

**Recommendations:**
- `1-5`: Simple functions (ideal)
- `6-10`: Moderate complexity (acceptable)
- `11-15`: Complex functions (should refactor)
- `>15`: Very complex (high priority for refactoring)

#### `--file-size-limit`
**Type:** Integer  
**Default:** `300`  
**Purpose:** Maximum lines per file

```bash
@tech-debt . --file-size-limit 250
```

**Guidelines:**
- `<100`: Very focused files
- `100-300`: Well-sized files (recommended)
- `300-500`: Large files (consider splitting)
- `>500`: Very large files (should split)

### Architecture Reviewer Parameters

#### `--detect-violations`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Find architecture rule violations

```bash
@arch-review . --detect-violations
```

#### `--suggest-improvements`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Generate improvement suggestions

```bash
@arch-review . --suggest-improvements
```

#### `--style`
**Type:** Enum  
**Values:** `mvc`, `hexagonal`, `ddd`, `microservices`, `auto`  
**Default:** `auto`  
**Purpose:** Architecture style to validate against

```bash
@arch-review . --style hexagonal
```

#### `--visualize`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Generate architecture diagrams

```bash
@arch-review . --visualize
```

#### `--output-format`
**Type:** Enum  
**Values:** `mermaid`, `plantuml`, `graphviz`  
**Default:** `mermaid`  
**Purpose:** Diagram format (when visualizing)

```bash
@arch-review . --visualize --output-format plantuml
```

#### `--depth`
**Type:** Enum  
**Values:** `shallow`, `standard`, `deep`  
**Default:** `standard`  
**Purpose:** Analysis depth

```bash
@arch-review . --depth deep
```

**Levels:**
- `shallow`: High-level overview only
- `standard`: Balanced detail level
- `deep`: Comprehensive analysis (slower)

### Test Generator Parameters

#### `--only-untested`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Generate tests only for untested code

```bash
@test-gen . --only-untested
```

#### `--coverage-target`
**Type:** Integer (0-100)  
**Default:** `80`  
**Purpose:** Target coverage percentage

```bash
@test-gen . --coverage-target 90
```

#### `--test-types`
**Type:** Comma-separated list  
**Values:** `unit`, `integration`, `e2e`, `all`  
**Default:** `unit`

```bash
@test-gen . --test-types unit,integration
```

#### `--framework`
**Type:** String  
**Default:** Auto-detected  
**Purpose:** Test framework to use

```bash
@test-gen . --framework jest
```

**Supported frameworks:**
- `jest`, `mocha`, `vitest` (JavaScript/TypeScript)
- `pytest`, `unittest` (Python)
- `phpunit` (PHP)
- `rspec` (Ruby)

#### `--mock-strategy`
**Type:** Enum  
**Values:** `minimal`, `realistic`, `comprehensive`  
**Default:** `realistic`

```bash
@test-gen . --mock-strategy comprehensive
```

#### `--update-existing`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Update existing test files

```bash
@test-gen . --update-existing
```

### Performance Optimizer Parameters

#### `--target`
**Type:** Comma-separated list  
**Values:** `frontend`, `backend`, `database`, `all`  
**Default:** `all`

```bash
@perf-optimize . --target frontend,database
```

#### `--bundle-analysis`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Analyze bundle size and composition

```bash
@perf-optimize . --bundle-analysis
```

#### `--suggest-indexes`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Suggest database indexes

```bash
@perf-optimize . --suggest-indexes
```

#### `--cache-strategy`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Develop caching recommendations

```bash
@perf-optimize . --cache-strategy
```

#### `--baseline`
**Type:** String (filename)  
**Purpose:** Performance baseline file

```bash
@perf-optimize . --baseline performance-baseline.json
```

### Migration Assistant Parameters

#### `--from`
**Type:** String (version or framework)  
**Required for migrations**

```bash
@migrate --from react@17
```

#### `--to`
**Type:** String (version or framework)  
**Required for migrations**

```bash
@migrate --to react@18
```

#### `--pattern`
**Type:** String  
**Purpose:** Migration pattern

```bash
@migrate --pattern class-to-hooks
```

**Common patterns:**
- `class-to-hooks` (React)
- `api-v1-to-v2` (API migrations)
- `js-to-ts` (JavaScript to TypeScript)

#### `--incremental`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Gradual migration approach

```bash
@migrate --incremental
```

#### `--compatibility-check`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Check compatibility before migration

```bash
@migrate --compatibility-check
```

#### `--rollback-plan`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Generate rollback procedures

```bash
@migrate --rollback-plan
```

### Refactor Parameters

#### `--type`
**Refactor Types:**
- `extract-method`: Extract repeated code into methods
- `extract-component`: Break down large components
- `reduce-complexity`: Simplify complex functions
- `consolidate-utils`: Merge duplicate utilities
- `organize-imports`: Clean up imports
- `remove-dead-code`: Remove unused code
- `split-file`: Split large files
- `standardize-patterns`: Fix inconsistent patterns
- `auto`: Auto-detect best approach

#### `--max-function-size`
**Type:** Integer  
**Default:** `30`

```bash
@refactor . --max-function-size 20
```

#### `--max-file-size`
**Type:** Integer  
**Default:** `300`

```bash
@refactor . --max-file-size 250
```

#### `--max-complexity`
**Type:** Integer  
**Default:** `10`

```bash
@refactor . --max-complexity 8
```

#### `--create-shared`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Create shared utilities from duplicates

```bash
@refactor . --create-shared
```

#### `--preserve-structure`
**Type:** Boolean  
**Default:** `false`  
**Purpose:** Maintain current file structure

```bash
@refactor . --preserve-structure
```

### New Feature Parameters

#### `--feature`
**Type:** String (quoted description)  
**Required**

```bash
@new-feature --feature "user dashboard with charts"
```

#### `--location`
**Type:** String (path)  
**Required**

```bash
@new-feature --location src/pages/
```

#### `--type`
**Feature Types:**
- `component`: UI component
- `service`: Business logic service
- `page`: Full page implementation
- `api`: API endpoint
- `full-stack`: Complete feature

#### `--tech-stack`
**Type:** String  
**Default:** Auto-detected

```bash
@new-feature --tech-stack react
```

#### `--example-files`
**Type:** Comma-separated paths

```bash
@new-feature --example-files "src/components/UserCard.tsx,src/pages/ProfilePage.tsx"
```

#### `--style-guide`
**Type:** String (path or name)

```bash
@new-feature --style-guide .eslintrc.js
```

#### `--test-first`
**Type:** Boolean  
**Default:** `false`

```bash
@new-feature --test-first
```

## Configuration File Parameters

### Configuration Priority

1. Command line arguments (highest priority)
2. Project configuration file (.command.config.js)
3. User configuration file (~/.claude/config.js)
4. System defaults (lowest priority)

### Configuration File Format

```javascript
// .techdebt.config.js
module.exports = {
  // Override any command line parameter
  threshold: 'aggressive',
  autoFix: 'safe',
  maxChanges: 25,
  
  // Command-specific settings
  includePaths: ['src/**/*.{js,ts}'],
  excludePaths: ['**/*.test.js'],
  
  // Thresholds and limits
  thresholds: {
    maxFileLines: 250,
    maxComplexity: 8,
    similarity: 0.8
  }
};
```

---

*For syntax examples, see [Command Syntax Reference](./command-syntax.md).*