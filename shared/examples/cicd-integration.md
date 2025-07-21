# CI/CD Integration Templates

This file contains reusable CI/CD pipeline examples that can be parameterized for different sub-agent commands and platforms.

## GitHub Actions Templates

### Basic Quality Check Job
```yaml
{{JOB_NAME}}:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '18'
    - name: Install Claude Code
      run: npm install -g @anthropic-ai/claude-code
    - name: {{STEP_NAME}}
      run: |
        {{COMMAND}} {{ARGS}}
        {{EXIT_CONDITION}}
```

### Multi-Command Quality Pipeline
```yaml
code-quality:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '18'
    - name: Install Claude Code
      run: npm install -g @anthropic-ai/claude-code
    - name: Run Code Quality Checks
      run: |
        {{COMMAND_1}} {{ARGS_1}}
        {{COMMAND_2}} {{ARGS_2}}
        {{COMMAND_3}} {{ARGS_3}}
    - name: Upload Reports
      uses: actions/upload-artifact@v4
      if: always()
      with:
        name: quality-reports
        path: |
          {{REPORT_PATHS}}
```

### Feature Generation with Inputs
```yaml
generate-feature:
  runs-on: ubuntu-latest
  inputs:
    feature:
      description: 'Feature description'
      required: true
    location:
      description: 'Feature location'
      required: false
      default: 'src/features'
  steps:
    - uses: actions/checkout@v4
    - name: Generate Feature
      run: |
        npx claude-code {{COMMAND}} \
          --feature "${{ inputs.feature }}" \
          --location "${{ inputs.location }}" \
          {{ADDITIONAL_ARGS}}
```

## GitLab CI Templates

### Basic Pipeline Job
```yaml
{{JOB_NAME}}:
  stage: {{STAGE}}
  image: node:18
  before_script:
    - npm install -g @anthropic-ai/claude-code
  script:
    - {{COMMAND}} {{ARGS}}
    - {{VALIDATION_SCRIPT}}
  artifacts:
    paths:
      - {{ARTIFACT_PATHS}}
    expire_in: 1 week
  only:
    - {{BRANCH_RULES}}
```

### Quality Gate Pipeline
```yaml
code-quality:
  stage: test
  image: node:18
  before_script:
    - npm install -g @anthropic-ai/claude-code
  script:
    - echo "Running code quality checks..."
    - {{COMMAND_1}} {{ARGS_1}}
    - echo "{{COMMAND_1_NAME}} completed with score: $(cat {{SCORE_FILE_1}})"
    - {{COMMAND_2}} {{ARGS_2}}
    - echo "{{COMMAND_2_NAME}} completed"
  artifacts:
    reports:
      junit: reports/*.xml
    paths:
      - reports/
      - {{OUTPUT_DIRS}}
  only:
    - merge_requests
    - main
```

### Parallel Analysis Jobs
```yaml
.quality_template: &quality_template
  stage: analysis
  image: node:18
  before_script:
    - npm install -g @anthropic-ai/claude-code
  only:
    - merge_requests

tech-debt-analysis:
  <<: *quality_template
  script:
    - sub-agent-tech-debt-finder-fixer --dry-run
    - echo "Tech debt score: $(cat tech-debt-score.txt)"

architecture-review:
  <<: *quality_template
  script:
    - sub-agent-architecture-reviewer --detect-violations
    - if [ $? -ne 0 ]; then exit 1; fi
```

## Jenkins Pipeline Templates

### Declarative Pipeline
```groovy
pipeline {
    agent any
    
    stages {
        stage('Setup') {
            steps {
                sh 'npm install -g @anthropic-ai/claude-code'
            }
        }
        
        stage('{{STAGE_NAME}}') {
            steps {
                sh '''
                    {{COMMAND}} {{ARGS}}
                    {{VALIDATION}}
                '''
            }
            post {
                always {
                    archiveArtifacts artifacts: '{{ARTIFACTS}}', allowEmptyArchive: true
                }
            }
        }
    }
}
```

### Scripted Pipeline
```groovy
node {
    stage('Checkout') {
        checkout scm
    }
    
    stage('Setup') {
        sh 'npm install -g @anthropic-ai/claude-code'
    }
    
    stage('{{STAGE_NAME}}') {
        try {
            sh '{{COMMAND}} {{ARGS}}'
            {{SUCCESS_ACTIONS}}
        } catch (Exception e) {
            {{FAILURE_ACTIONS}}
            throw e
        }
    }
}
```

## Platform-Specific Examples

### GitHub Actions - Tech Debt Finder
```yaml
tech-debt-check:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
    - name: Install Claude Code
      run: npm install -g @anthropic-ai/claude-code
    - name: Find Tech Debt
      run: |
        sub-agent-tech-debt-finder-fixer --dry-run
        echo "Tech debt score: $(cat tech-debt-score.txt)"
    - name: Comment PR
      uses: actions/github-script@v7
      if: github.event_name == 'pull_request'
      with:
        script: |
          const fs = require('fs');
          const score = fs.readFileSync('tech-debt-score.txt', 'utf8');
          github.rest.issues.createComment({
            issue_number: context.issue.number,
            owner: context.repo.owner,
            repo: context.repo.repo,
            body: `## Tech Debt Analysis\n\nTech Debt Score: ${score}`
          });
```

### GitLab CI - Architecture Review
```yaml
architecture-check:
  stage: test
  script:
    - sub-agent-architecture-reviewer --detect-violations
    - if [ $? -ne 0 ]; then exit 1; fi
  artifacts:
    paths:
      - architecture-report.md
      - diagrams/
    expire_in: 1 week
  only:
    - merge_requests
```

### Azure DevOps - Performance Optimization
```yaml
- task: NodeTool@0
  displayName: 'Use Node 18'
  inputs:
    versionSpec: '18'

- script: npm install -g @anthropic-ai/claude-code
  displayName: 'Install Claude Code'

- script: |
    sub-agent-performance-optimizer --target all --dry-run
    echo "Performance score: $(cat performance-score.txt)"
  displayName: 'Performance Analysis'

- task: PublishTestResults@2
  inputs:
    testResultsFiles: 'reports/*.xml'
  condition: always()
```

## Common Parameter Combinations

### Tech Debt Finder & Fixer
- **Command**: `sub-agent-tech-debt-finder-fixer`
- **Dry Run Args**: `--dry-run`
- **Non-Interactive Args**: `--interactive false --auto-fix safe`
- **Score File**: `tech-debt-score.txt`

### Architecture Reviewer
- **Command**: `sub-agent-architecture-reviewer`
- **Violation Check Args**: `--detect-violations`
- **Report Paths**: `architecture-report.md`, `diagrams/`

### Test Generator
- **Command**: `sub-agent-test-generator`
- **Coverage Args**: `--only-untested`
- **Report Paths**: `coverage/`, `test-reports/`

### Performance Optimizer
- **Command**: `sub-agent-performance-optimizer`
- **Analysis Args**: `--target all --dry-run`
- **Score File**: `performance-score.txt`

### Refactor
- **Command**: `sub-agent-refactor`
- **Safe Args**: `src/ --dry-run`
- **Exit Condition**: `if [ $? -ne 0 ]; then echo "Refactoring opportunities found"; exit 1; fi`

## Integration Best Practices

1. **Always use dry-run mode** in CI/CD to prevent automatic changes
2. **Set appropriate exit codes** for pipeline failure conditions
3. **Archive reports and artifacts** for later analysis
4. **Use parallel jobs** when running multiple analysis commands
5. **Add score thresholds** to fail builds on quality degradation
6. **Cache dependencies** for faster pipeline execution
7. **Use conditional execution** based on changed files or branches