# Sub-Agent Dynamic Coordinator

An intelligent meta-command that automatically selects and coordinates the optimal combination of specialized agents based on project characteristics, historical performance, and specific analysis goals.

## Command Syntax

```bash
sub-agent-dynamic-coordinator [target] [options]

# Aliases
@dynamic-coordinator [target] [options]
@auto-agent [target] [options]
@sdc [target] [options]
```

## Overview

The Dynamic Coordinator represents the next evolution in parallel sub-agent systems. Instead of manually choosing which agents to run, it:

1. **Analyzes project characteristics** (framework, size, complexity, history)
2. **Selects optimal agent combinations** based on learned patterns
3. **Coordinates parallel execution** with intelligent resource allocation
4. **Adapts strategies** based on real-time performance and results

## Parameters

### Target
- `target` - Directory, file, or glob pattern to analyze (defaults to current directory)

### Core Options
- `--goal` - Analysis goal: `tech-debt`, `architecture`, `performance`, `testing`, `migration`, `comprehensive`
- `--framework` - Framework hint: `react`, `vue`, `angular`, `laravel`, `django`, `spring`, `auto`
- `--size` - Project size hint: `small`, `medium`, `large`, `auto`
- `--priority` - Priority focus: `speed`, `thoroughness`, `cost`, `balance`

### Execution Control
- `--dry-run` - Show selected agents and coordination plan without executing
- `--interactive` - Review agent selection before execution
- `--max-agents` - Maximum number of parallel agents (default: 5)
- `--timeout` - Total execution timeout in milliseconds
- `--memory` - Enable memory-enhanced selection (requires memory system)

### Agent Selection Control
- `--include-agents` - Force inclusion of specific agent types
- `--exclude-agents` - Exclude specific agent types
- `--agent-strategy` - Selection strategy: `conservative`, `balanced`, `aggressive`
- `--specialization` - Specialization level: `generalist`, `specialist`, `expert`

### Learning and Adaptation
- `--learn-mode` - Learning aggressiveness: `none`, `passive`, `active`, `aggressive`
- `--feedback-mode` - Enable post-execution feedback collection
- `--use-history` - Apply historical performance data
- `--context-aware` - Use project context for agent selection

## Examples

### Basic Usage
```bash
# Auto-analyze with intelligent agent selection
@dynamic-coordinator src/

# Focus on specific goal
@dynamic-coordinator . --goal tech-debt --priority speed

# Framework-specific optimization
@dynamic-coordinator . --framework react --goal performance
```

### Advanced Usage
```bash
# Comprehensive analysis with learning
@dynamic-coordinator . --goal comprehensive \
  --memory --learn-mode active \
  --feedback-mode --context-aware

# Large project optimization
@dynamic-coordinator . --size large \
  --max-agents 7 --priority thoroughness \
  --agent-strategy aggressive

# Migration preparation
@dynamic-coordinator . --goal migration \
  --framework laravel --specialization expert \
  --include-agents architecture,tech-debt
```

## Agent Selection Algorithm

### Phase 1: Project Characterization

The coordinator begins by analyzing project characteristics:

```
Project Analysis Agent:
"Analyze the target codebase to determine:
1. Framework and technology stack
2. Project size and complexity metrics
3. Code quality indicators
4. Architecture patterns in use
5. Test coverage and quality
6. Recent change patterns (if git available)
7. Performance characteristics

Return structured project profile for agent selection."
```

**Project Profile Output:**
```json
{
  "framework": {
    "primary": "react",
    "version": "18.2.0",
    "confidence": 0.95
  },
  "size": {
    "category": "medium",
    "files": 1247,
    "linesOfCode": 45230,
    "components": 89
  },
  "complexity": {
    "average": 7.2,
    "max": 23,
    "distribution": "normal"
  },
  "quality": {
    "testCoverage": 0.67,
    "duplicateRatio": 0.12,
    "techDebtRatio": 0.18
  },
  "architecture": {
    "style": "component-based",
    "patterns": ["hooks", "context", "custom-hooks"],
    "layering": "moderate"
  }
}
```

### Phase 2: Goal-Specific Agent Selection

Based on the analysis goal, different agent combinations are optimal:

#### Tech Debt Analysis Goal
```typescript
const techDebtAgentMatrix = {
  small: {
    conservative: ['duplicate-detector', 'complexity-analyzer'],
    balanced: ['duplicate-detector', 'complexity-analyzer', 'dead-code-hunter'],
    aggressive: ['duplicate-detector', 'complexity-analyzer', 'dead-code-hunter', 'pattern-consistency']
  },
  medium: {
    conservative: ['duplicate-detector', 'complexity-analyzer', 'pattern-consistency'],
    balanced: ['duplicate-detector', 'complexity-analyzer', 'pattern-consistency', 'dead-code-hunter'],
    aggressive: ['duplicate-detector', 'complexity-analyzer', 'pattern-consistency', 'dead-code-hunter', 'type-coverage']
  },
  large: {
    conservative: ['duplicate-detector', 'complexity-analyzer', 'pattern-consistency', 'dead-code-hunter'],
    balanced: ['duplicate-detector', 'complexity-analyzer', 'pattern-consistency', 'dead-code-hunter', 'type-coverage'],
    aggressive: ['duplicate-detector', 'complexity-analyzer', 'pattern-consistency', 'dead-code-hunter', 'type-coverage', 'dependency-analyzer']
  }
};
```

#### Performance Analysis Goal
```typescript
const performanceAgentMatrix = {
  react: ['bundle-analyzer', 'component-performance', 'render-optimizer'],
  vue: ['bundle-analyzer', 'component-performance', 'reactivity-optimizer'],
  laravel: ['query-optimizer', 'route-performance', 'cache-analyzer'],
  django: ['query-optimizer', 'view-performance', 'orm-analyzer']
};
```

#### Architecture Review Goal
```typescript
const architectureAgentMatrix = {
  'component-based': ['dependency-mapper', 'layer-analyzer', 'pattern-compliance'],
  'microservices': ['service-analyzer', 'communication-mapper', 'boundary-checker'],
  'monolithic': ['module-analyzer', 'coupling-detector', 'separation-evaluator']
};
```

### Phase 3: Memory-Enhanced Selection

If memory system is available, enhance selection with historical data:

```
Memory-Enhanced Selection Agent:
"Based on the project profile and analysis goal, query the memory system for:
1. Similar projects and their optimal agent combinations
2. Historical performance data for different agent configurations
3. User feedback patterns for this project type
4. Previous analysis results that can inform agent selection

Adjust agent selection matrix based on learned optimizations."
```

### Phase 4: Resource Optimization

Optimize agent selection for available resources:

```typescript
interface ResourceConstraints {
  maxParallelAgents: number;
  timeoutMs: number;
  memoryLimit: number;
  priorityMode: 'speed' | 'thoroughness' | 'cost' | 'balance';
}

function optimizeAgentSelection(
  selectedAgents: AgentType[],
  constraints: ResourceConstraints
): OptimizedAgentConfiguration {
  // Consider execution time, resource usage, and interdependencies
  return {
    primaryAgents: AgentType[],
    secondaryAgents: AgentType[],
    executionOrder: ExecutionPlan,
    resourceAllocation: ResourcePlan
  };
}
```

## Coordination Strategies

### Parallel Execution Coordination

```
Coordination Agent:
"Execute the selected agents in parallel with intelligent coordination:

Primary Agents (Execute immediately in parallel):
- {selected_primary_agents}

Secondary Agents (Execute based on primary results):
- {selected_secondary_agents}

Resource Management:
- Maximum concurrent agents: {max_agents}
- Timeout per agent: {agent_timeout}
- Memory allocation: {memory_per_agent}

Error Handling:
- Continue with partial results if some agents fail
- Prioritize most critical agents for resource allocation
- Provide graceful degradation if resources are constrained"
```

### Dynamic Resource Allocation

```typescript
class DynamicResourceManager {
  allocateResources(agents: AgentConfiguration[]): ResourceAllocation {
    return {
      cpuAllocation: this.distributeCPU(agents),
      memoryAllocation: this.distributeMemory(agents),
      timeoutAllocation: this.distributeTime(agents),
      priorityOrder: this.calculatePriority(agents)
    };
  }
  
  private distributeCPU(agents: AgentConfiguration[]): CPUAllocation {
    // More CPU for computationally intensive agents
    const cpuWeights = {
      'duplicate-detector': 3,
      'complexity-analyzer': 2,
      'dependency-mapper': 4,
      'pattern-consistency': 2
    };
    return this.weightedDistribution(agents, cpuWeights);
  }
}
```

### Adaptive Execution

```typescript
class AdaptiveExecutor {
  async executeWithAdaptation(
    agentConfig: AgentConfiguration,
    constraints: ResourceConstraints
  ): Promise<ExecutionResults> {
    
    const monitor = new ExecutionMonitor();
    const results: ExecutionResults = {};
    
    // Start primary agents
    const primaryPromises = this.startPrimaryAgents(agentConfig.primary);
    
    // Monitor progress and adapt
    while (!this.allComplete(primaryPromises)) {
      const status = await monitor.checkStatus(primaryPromises);
      
      if (status.needsAdaptation) {
        await this.adaptExecution(status, agentConfig);
      }
      
      await this.sleep(1000); // Check every second
    }
    
    return this.consolidateResults(results);
  }
  
  private async adaptExecution(
    status: ExecutionStatus,
    config: AgentConfiguration
  ): Promise<void> {
    if (status.slowAgents.length > 0) {
      // Reallocate resources to struggling agents
      await this.reallocateResources(status.slowAgents);
    }
    
    if (status.failedAgents.length > 0) {
      // Start backup agents for failed ones
      await this.startBackupAgents(status.failedAgents, config);
    }
  }
}
```

## Agent Selection Examples

### React E-commerce Project

**Project Profile:**
- Framework: React 18 with TypeScript
- Size: 1,247 files, 45K lines
- Complexity: High component nesting
- Goal: Tech debt cleanup before feature development

**Selected Agents:**
```
Primary Agents (Parallel):
1. React-Specialized Duplicate Detector
   - Focus: Component patterns, hook duplications
   - Timeout: 3 minutes
   
2. Component Complexity Analyzer  
   - Focus: React component complexity, prop drilling
   - Timeout: 4 minutes
   
3. TypeScript Pattern Consistency
   - Focus: Type definitions, interface patterns
   - Timeout: 2 minutes

Secondary Agents (Based on primary results):
4. React Performance Analyzer
   - Trigger: If complexity > 15 in components
   
5. Test Coverage Analyzer
   - Trigger: If duplicate components found
```

### Laravel API Project

**Project Profile:**
- Framework: Laravel 9 with PHP 8.1
- Size: 567 files, 23K lines
- Complexity: Moderate with good separation
- Goal: Performance optimization for high load

**Selected Agents:**
```
Primary Agents (Parallel):
1. Database Query Optimizer
   - Focus: Eloquent queries, N+1 problems
   - Timeout: 5 minutes
   
2. Route Performance Analyzer
   - Focus: Route complexity, middleware chains
   - Timeout: 3 minutes
   
3. Laravel Pattern Compliance
   - Focus: Service patterns, repository patterns
   - Timeout: 2 minutes

Secondary Agents:
4. Cache Strategy Analyzer
   - Trigger: If slow queries detected
   
5. API Response Optimizer
   - Trigger: If large response payloads found
```

## Memory Integration

### Learning Agent Performance

```typescript
interface AgentPerformanceRecord {
  agentType: string;
  projectProfile: ProjectProfile;
  executionTime: number;
  findingsQuality: number;
  userSatisfaction: number;
  resourceUsage: ResourceUsage;
}

class PerformanceLearning {
  async recordExecution(
    agentType: string,
    profile: ProjectProfile,
    performance: PerformanceMetrics
  ): Promise<void> {
    const record: AgentPerformanceRecord = {
      agentType,
      projectProfile: profile,
      executionTime: performance.duration,
      findingsQuality: performance.qualityScore,
      userSatisfaction: performance.userFeedback,
      resourceUsage: performance.resources
    };
    
    await this.memory.storePerformance(record);
    await this.updateAgentRankings(agentType, profile, performance);
  }
  
  async getOptimalAgents(
    profile: ProjectProfile,
    goal: AnalysisGoal
  ): Promise<AgentType[]> {
    const similarProjects = await this.memory.findSimilarProjects(profile);
    const performanceData = await this.memory.getPerformanceData(similarProjects);
    
    return this.rankAgentsByPerformance(performanceData, goal);
  }
}
```

### Context-Aware Selection

```typescript
class ContextAwareSelector {
  async selectAgents(
    profile: ProjectProfile,
    goal: AnalysisGoal,
    context: ProjectContext
  ): Promise<AgentConfiguration> {
    
    // Base selection from project profile
    const baseAgents = this.getBaseAgents(profile, goal);
    
    // Enhance based on context
    const contextEnhanced = await this.enhanceWithContext(baseAgents, context);
    
    // Apply memory optimizations
    const memoryOptimized = await this.applyMemoryLearning(contextEnhanced, profile);
    
    // Final resource optimization
    return this.optimizeForResources(memoryOptimized);
  }
  
  private async enhanceWithContext(
    agents: AgentType[],
    context: ProjectContext
  ): Promise<AgentType[]> {
    
    if (context.recentMigration) {
      agents.push('migration-impact-analyzer');
    }
    
    if (context.highBugRate) {
      agents.push('quality-risk-analyzer');
    }
    
    if (context.performanceIssues) {
      agents.push('performance-bottleneck-detector');
    }
    
    return agents;
  }
}
```

## Output and Reporting

### Execution Report

```json
{
  "coordinatorVersion": "1.0.0",
  "executionId": "exec_20240120_143022",
  "projectProfile": {
    "framework": "react",
    "size": "medium",
    "complexity": 7.2
  },
  "agentSelection": {
    "strategy": "balanced",
    "selectedAgents": [
      {
        "type": "duplicate-detector",
        "specialization": "react-components",
        "priority": 1,
        "estimatedTime": 180
      },
      {
        "type": "complexity-analyzer", 
        "specialization": "component-complexity",
        "priority": 1,
        "estimatedTime": 240
      }
    ],
    "rejectedAgents": [
      {
        "type": "database-analyzer",
        "reason": "not-applicable-to-framework"
      }
    ]
  },
  "execution": {
    "startTime": "2024-01-20T14:30:22Z",
    "endTime": "2024-01-20T14:34:15Z",
    "totalDuration": 233000,
    "parallelEfficiency": 0.85
  },
  "results": {
    "findings": 47,
    "criticalIssues": 3,
    "recommendations": 23,
    "estimatedEffort": "12-16 hours"
  },
  "performance": {
    "resourceUsage": {
      "maxMemory": "512MB",
      "avgCPU": "65%"
    },
    "agentPerformance": [
      {
        "agent": "duplicate-detector",
        "duration": 195000,
        "findings": 12,
        "efficiency": 0.92
      }
    ]
  },
  "learning": {
    "patternsLearned": 5,
    "performanceImprovement": "+15%",
    "recommendationAccuracy": 0.89
  }
}
```

### Selection Explanation

```markdown
## Agent Selection Report

### Project Analysis
- **Framework**: React 18.2.0 (95% confidence)
- **Size**: Medium (1,247 files, 45K LOC)
- **Complexity**: Moderate-High (avg: 7.2, max: 23)
- **Architecture**: Component-based with hooks

### Goal: Tech Debt Cleanup

### Selected Agent Strategy: Balanced

#### Primary Agents (Parallel Execution)
1. **React-Specialized Duplicate Detector** (Priority: High)
   - **Why**: High component count suggests potential duplication
   - **Focus**: Component patterns, custom hooks, utility functions
   - **Estimated Time**: 3 minutes

2. **Component Complexity Analyzer** (Priority: High)  
   - **Why**: High max complexity (23) indicates problematic components
   - **Focus**: Component props, nesting levels, lifecycle complexity
   - **Estimated Time**: 4 minutes

3. **TypeScript Pattern Consistency** (Priority: Medium)
   - **Why**: TypeScript project benefits from type consistency analysis
   - **Focus**: Interface definitions, type usage patterns
   - **Estimated Time**: 2 minutes

#### Secondary Agents (Conditional)
4. **React Performance Analyzer** 
   - **Trigger**: If components with complexity > 15 found
   - **Purpose**: Identify performance impact of complex components

5. **Test Coverage Analyzer**
   - **Trigger**: If duplicate components detected
   - **Purpose**: Ensure tests exist before refactoring duplicates

### Excluded Agents
- **Database Analyzer**: Not applicable (frontend-only project)
- **API Optimizer**: No backend API detected
- **Cache Analyzer**: No caching patterns found

### Memory Learning Applied
- Similar React projects showed 15% better results with this combination
- Component complexity analysis proved high-value for this project size
- TypeScript consistency checking reduced false positives by 23%

### Resource Allocation
- **Max Parallel Agents**: 3 (optimal for medium projects)
- **Total Estimated Time**: 4 minutes (longest-running agent)
- **Memory Usage**: ~512MB peak
- **CPU Utilization**: ~65% average
```

This Dynamic Coordinator represents a significant evolution in parallel sub-agent systems, providing intelligent, adaptive agent selection that learns and improves over time while maintaining the reliability and effectiveness of our specialized approach.

---

*The Dynamic Coordinator bridges the gap between manual agent selection and fully autonomous AI orchestration, providing intelligent assistance while maintaining user control and transparency.*