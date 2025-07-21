# Git Hooks Integration Templates

This file contains reusable Git hook examples that can be parameterized for different sub-agent commands and use cases.

## Pre-commit Hook Templates

### Basic Quality Check Hook
```bash
#!/bin/sh
# {{HOOK_DESCRIPTION}}

echo "Running {{COMMAND_NAME}}..."
{{COMMAND}} {{ARGS}}

if [ $? -ne 0 ]; then
    echo "❌ {{FAILURE_MESSAGE}}"
    echo "💡 {{SUGGESTION}}"
    exit 1
fi

echo "✅ {{SUCCESS_MESSAGE}}"
```

### Multi-Command Pre-commit Hook
```bash
#!/bin/sh
# Comprehensive code quality pre-commit hook

echo "🔍 Running code quality checks..."

# {{COMMAND_1_DESCRIPTION}}
echo "Running {{COMMAND_1_NAME}}..."
{{COMMAND_1}} {{ARGS_1}}
if [ $? -ne 0 ]; then
    echo "❌ {{COMMAND_1_FAILURE}}"
    exit 1
fi

# {{COMMAND_2_DESCRIPTION}}
echo "Running {{COMMAND_2_NAME}}..."
{{COMMAND_2}} {{ARGS_2}}
if [ $? -ne 0 ]; then
    echo "❌ {{COMMAND_2_FAILURE}}"
    exit 1
fi

echo "✅ All quality checks passed!"
```

### Conditional Pre-commit Hook
```bash
#!/bin/sh
# Run checks only on relevant file changes

# Get list of changed files
CHANGED_FILES=$(git diff --cached --name-only)

# {{CONDITION_DESCRIPTION}}
if echo "$CHANGED_FILES" | grep -E "{{FILE_PATTERN}}" > /dev/null; then
    echo "{{FILE_TYPE}} files changed, running {{COMMAND_NAME}}..."
    {{COMMAND}} {{ARGS}}
    
    if [ $? -ne 0 ]; then
        echo "❌ {{FAILURE_MESSAGE}}"
        exit 1
    fi
else
    echo "No {{FILE_TYPE}} files changed, skipping {{COMMAND_NAME}}"
fi
```

## Pre-push Hook Templates

### Architecture Validation Hook
```bash
#!/bin/sh
# Validate architecture before pushing

protected_branch='{{PROTECTED_BRANCH}}'
current_branch=$(git symbolic-ref HEAD | sed -e 's,.*/\(.*\),\1,')

if [ $protected_branch = $current_branch ]; then
    echo "🏗️  Validating architecture for $protected_branch branch..."
    {{COMMAND}} {{ARGS}}
    
    if [ $? -ne 0 ]; then
        echo "❌ Architecture validation failed"
        echo "💡 Fix architecture violations before pushing to $protected_branch"
        exit 1
    fi
    
    echo "✅ Architecture validation passed"
fi
```

### Comprehensive Pre-push Hook
```bash
#!/bin/sh
# Comprehensive checks before push

remote="$1"
url="$2"

zero=0000000000000000000000000000000000000000

while read local_ref local_sha remote_ref remote_sha
do
    if [ "$local_sha" = $zero ]; then
        # Handle delete
        :
    else
        if [ "$remote_sha" = $zero ]; then
            # New branch, examine all commits
            range="$local_sha"
        else
            # Update to existing branch, examine new commits
            range="$remote_sha..$local_sha"
        fi

        # Check for commits
        if [ -n "$range" ]; then
            echo "🔍 Running quality checks on commits $range..."
            
            # {{COMMAND_1_DESCRIPTION}}
            {{COMMAND_1}} {{ARGS_1}}
            if [ $? -ne 0 ]; then
                echo "❌ {{COMMAND_1_FAILURE}}"
                exit 1
            fi
            
            # {{COMMAND_2_DESCRIPTION}}
            {{COMMAND_2}} {{ARGS_2}}
            if [ $? -ne 0 ]; then
                echo "❌ {{COMMAND_2_FAILURE}}"
                exit 1
            fi
        fi
    fi
done

echo "✅ All pre-push checks passed!"
```

## Post-commit Hook Templates

### Documentation Update Hook
```bash
#!/bin/sh
# Update documentation after commit

echo "📝 Updating documentation..."
{{COMMAND}} {{ARGS}}

if [ $? -eq 0 ]; then
    echo "✅ Documentation updated successfully"
else
    echo "⚠️  Documentation update had issues (non-blocking)"
fi
```

### Metrics Collection Hook
```bash
#!/bin/sh
# Collect code quality metrics

echo "📊 Collecting code quality metrics..."

# {{METRIC_1_DESCRIPTION}}
{{COMMAND_1}} {{ARGS_1}} > {{OUTPUT_FILE_1}} 2>&1

# {{METRIC_2_DESCRIPTION}}
{{COMMAND_2}} {{ARGS_2}} > {{OUTPUT_FILE_2}} 2>&1

echo "✅ Metrics collected"
```

## Specific Command Examples

### Tech Debt Finder Pre-commit
```bash
#!/bin/sh
# Check for tech debt before commit

echo "🔍 Checking for tech debt..."
sub-agent-tech-debt-finder-fixer --dry-run --quiet

if [ $? -ne 0 ]; then
    echo "❌ Tech debt issues found"
    echo "💡 Run 'sub-agent-tech-debt-finder-fixer --interactive' to fix issues"
    echo "   Or use --no-verify to skip this check"
    exit 1
fi

echo "✅ No critical tech debt found"
```

### Architecture Review Pre-push
```bash
#!/bin/sh
# Validate architecture before push to main

protected_branch='main'
current_branch=$(git symbolic-ref HEAD | sed -e 's,.*/\(.*\),\1,')

if [ $protected_branch = $current_branch ]; then
    echo "🏗️  Validating architecture for main branch..."
    sub-agent-architecture-reviewer --detect-violations
    
    if [ $? -ne 0 ]; then
        echo "❌ Architecture violations detected"
        echo "💡 Fix violations before pushing to main"
        exit 1
    fi
    
    echo "✅ Architecture validation passed"
fi
```

### Test Coverage Pre-commit
```bash
#!/bin/sh
# Ensure test coverage for new code

echo "🧪 Checking test coverage..."

# Get changed files
CHANGED_FILES=$(git diff --cached --name-only --diff-filter=AM | grep -E '\.(js|ts|jsx|tsx)$' | grep -v '\.test\.' | grep -v '\.spec\.')

if [ -n "$CHANGED_FILES" ]; then
    echo "Checking test coverage for changed files..."
    for file in $CHANGED_FILES; do
        echo "  Checking $file..."
        sub-agent-test-generator "$file" --check-coverage-only
        if [ $? -ne 0 ]; then
            echo "❌ Missing tests for $file"
            echo "💡 Run 'sub-agent-test-generator $file' to generate tests"
            exit 1
        fi
    done
fi

echo "✅ Test coverage check passed"
```

### Performance Check Pre-push
```bash
#!/bin/sh
# Check for performance regressions

protected_branch='main'
current_branch=$(git symbolic-ref HEAD | sed -e 's,.*/\(.*\),\1,')

if [ $protected_branch = $current_branch ]; then
    echo "⚡ Checking for performance regressions..."
    sub-agent-performance-optimizer --target all --dry-run --baseline
    
    if [ $? -ne 0 ]; then
        echo "❌ Performance regression detected"
        echo "💡 Review performance report and optimize before pushing"
        exit 1
    fi
    
    echo "✅ No performance regressions detected"
fi
```

### Refactoring Opportunities Post-commit
```bash
#!/bin/sh
# Report refactoring opportunities after commit

echo "🔄 Analyzing refactoring opportunities..."
sub-agent-refactor . --dry-run --report-only > refactor-opportunities.txt

if [ -s refactor-opportunities.txt ]; then
    echo "💡 Refactoring opportunities found:"
    head -10 refactor-opportunities.txt
    echo "   See refactor-opportunities.txt for full report"
    echo "   Run 'sub-agent-refactor --interactive' to address them"
fi
```

## Installation Scripts

### Hook Installer Template
```bash
#!/bin/bash
# Install {{HOOK_TYPE}} hook for {{COMMAND_NAME}}

HOOK_DIR=".git/hooks"
HOOK_FILE="$HOOK_DIR/{{HOOK_TYPE}}"

# Create hooks directory if it doesn't exist
mkdir -p "$HOOK_DIR"

# Create the hook
cat > "$HOOK_FILE" << 'EOF'
{{HOOK_CONTENT}}
EOF

# Make it executable
chmod +x "$HOOK_FILE"

echo "✅ {{HOOK_TYPE}} hook installed for {{COMMAND_NAME}}"
echo "💡 Use --no-verify to skip hook when needed"
```

### Multi-hook Installer
```bash
#!/bin/bash
# Install multiple git hooks for code quality

HOOKS_DIR=".git/hooks"

# Ensure hooks directory exists
mkdir -p "$HOOKS_DIR"

# Install pre-commit hook
cat > "$HOOKS_DIR/pre-commit" << 'EOF'
#!/bin/sh
# Multi-command pre-commit quality check
{{PRE_COMMIT_CONTENT}}
EOF

# Install pre-push hook  
cat > "$HOOKS_DIR/pre-push" << 'EOF'
#!/bin/sh
# Pre-push architecture validation
{{PRE_PUSH_CONTENT}}
EOF

# Make hooks executable
chmod +x "$HOOKS_DIR/pre-commit"
chmod +x "$HOOKS_DIR/pre-push"

echo "✅ Git hooks installed successfully"
echo "   - pre-commit: Tech debt and quality checks"
echo "   - pre-push: Architecture validation"
echo ""
echo "💡 Use --no-verify to skip hooks when needed"
```

## Hook Management

### Hook Status Checker
```bash
#!/bin/bash
# Check status of installed hooks

HOOKS_DIR=".git/hooks"

echo "📋 Git Hooks Status:"
echo "===================="

for hook in pre-commit pre-push post-commit commit-msg; do
    if [ -f "$HOOKS_DIR/$hook" ]; then
        echo "✅ $hook: Installed"
        if [ -x "$HOOKS_DIR/$hook" ]; then
            echo "   Executable: Yes"
        else
            echo "   Executable: No (chmod +x needed)"
        fi
    else
        echo "❌ $hook: Not installed"
    fi
    echo ""
done
```

### Hook Uninstaller
```bash
#!/bin/bash
# Remove specific git hooks

HOOKS_DIR=".git/hooks"
HOOK_NAME="$1"

if [ -z "$HOOK_NAME" ]; then
    echo "Usage: $0 <hook-name>"
    echo "Available hooks: pre-commit, pre-push, post-commit, commit-msg"
    exit 1
fi

if [ -f "$HOOKS_DIR/$HOOK_NAME" ]; then
    rm "$HOOKS_DIR/$HOOK_NAME"
    echo "✅ $HOOK_NAME hook removed"
else
    echo "❌ $HOOK_NAME hook not found"
fi
```

## Usage Notes

1. **Place hooks in `.git/hooks/`** directory
2. **Make hooks executable** with `chmod +x`
3. **Use `--no-verify`** to skip hooks when needed
4. **Test hooks thoroughly** before deploying to team
5. **Keep hooks fast** to avoid slowing down commits
6. **Provide clear error messages** with suggestions
7. **Consider using tools like husky** for easier hook management