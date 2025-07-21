# Tutorial: Your First Parallel Analysis

In this tutorial, we'll experience true parallel sub-agent execution firsthand. We'll analyze a codebase using multiple concurrent agents and see how they coordinate to produce comprehensive insights.

**What we'll accomplish:** By the end of this tutorial, we'll have conducted a complete parallel analysis and understand how concurrent agents work together.

**What we'll learn:** The difference between sequential simulation and true parallel execution, and how to interpret multi-agent findings.

**Time required:** 15-20 minutes

## What We're Building

We're going to run a parallel tech debt analysis that launches 3 agents simultaneously:
- **Agent 1**: Finds duplicate code patterns
- **Agent 2**: Analyzes complexity metrics  
- **Agent 3**: Detects inconsistent patterns

Then we'll watch a synthesis agent consolidate their findings into actionable recommendations.

## Before We Start

**Prerequisites:**
- Claude Code installed and working
- A codebase to analyze (we'll use this repository, but any project works)
- Basic familiarity with command line

**What we won't need:**
- Deep understanding of AI or agent systems
- Programming experience (though it helps)
- Perfect codebase (messy code actually makes for better demos!)

## Step 1: Install the Parallel Command

First, we'll install the parallel tech debt finder command.

```bash
# Navigate to your Claude commands directory
cd ~/.claude/commands

# Copy the parallel command file
cp /path/to/subagent-example-script/sub-agent-tech-debt-finder-parallel.md .
```

**Let's verify it worked:**
```bash
ls -la | grep parallel
```

You should see `sub-agent-tech-debt-finder-parallel.md` in the list.

## Step 2: Choose Our Target

We'll analyze this very repository to see the parallel system examine itself.

```bash
# Navigate to a good test location
cd /path/to/subagent-example-script
```

**Take a moment to look around:**
```bash
ls -la
```

Notice we have several markdown files, a docs directory, and shared examples. This gives our agents plenty to analyze.

## Step 3: Launch the Parallel Analysis

Now for the exciting part - we'll launch our parallel analysis and watch multiple agents work simultaneously.

**Run the command:**
```bash
@tech-debt-parallel . --dry-run
```

**What we should see happening:**

1. **Agent Launch Phase** (simultaneous execution):
   ```
   [Task 1] Analyzing duplicates in .../
   [Task 2] Analyzing complexity in .../
   [Task 3] Analyzing patterns in .../
   ```

2. **Progress Updates** (as agents complete):
   ```
   [Task 1] ✅ Found 23 duplicate blocks across 4 files
   [Task 2] ✅ Identified 8 oversized files and 12 complex sections
   [Task 3] ✅ Detected 15 pattern inconsistencies
   ```

3. **Synthesis Phase**:
   ```
   [Synthesis] Consolidating findings from 3 parallel agents...
   [Synthesis] Creating unified tech debt report...
   ```

**Notice the timing:** All three analysis agents run at the same time, not one after another. This is true parallelism in action!

## Step 4: Examine the Results

The synthesis agent will present a consolidated report. Let's walk through what we're seeing.

**Priority Matrix Section:**
```
Quick Wins (High Impact, Low Effort):
- Standardize alias patterns (2 hours)
- Extract duplicate examples (1 hour)
- Unify parameter naming (1 hour)

Major Improvements (High Impact, Medium Effort):
- Split oversized files (4 hours)
- Consolidate agent patterns (3 hours)
```

**What this tells us:** The parallel analysis found both immediate opportunities (quick wins) and larger architectural improvements.

**Dependency Analysis Section:**
```
Prerequisites:
1. Create shared directory structure
2. Establish naming conventions

Parallel Opportunities:
- Alias standardization + Parameter naming (can run together)
- Multiple file operations (can run concurrently)
```

**What this shows us:** The synthesis agent identified which tasks can be done in parallel vs. which need to happen in sequence.

## Step 5: Compare with Sequential Analysis

Let's understand what we just experienced by comparing it to how this would work sequentially.

**Sequential approach (old way):**
```
Agent 1 analyzes → 3 minutes
Agent 2 analyzes → 3 minutes  
Agent 3 analyzes → 3 minutes
Manual synthesis → 2 minutes
Total: 11 minutes
```

**Parallel approach (what we just did):**
```
Agents 1,2,3 analyze concurrently → 3 minutes
Automatic synthesis → 2 minutes
Total: 5 minutes (55% faster!)
```

**The key insight:** We didn't just save time - we got better results because each agent could focus purely on their specialty without waiting for others.

## Step 6: Understanding the Agent Specialization

Let's examine what each agent found and why their specialization matters.

**Agent 1 (Duplicate Detector) found:**
- Repeated integration examples across multiple files
- Similar command structure patterns
- Identical configuration sections

**Agent 2 (Complexity Analyzer) found:**  
- Files over 500 lines that should be split
- Commands with too many parameters
- Over-complex agent orchestrations

**Agent 3 (Pattern Detector) found:**
- Inconsistent alias naming schemes
- Mixed parameter conventions
- Different example formatting styles

**Why specialization works:** Each agent brought deep expertise to their domain. A general-purpose agent would have found surface-level issues, but these specialists identified architectural problems.

## Step 7: See the Synthesis Magic

The most impressive part is how the synthesis agent combined these findings.

**Individual findings:** 3 separate lists of technical issues

**Synthesized output:** 
- Unified priority matrix
- Dependency relationships between fixes
- Realistic time estimates
- Risk assessments
- Implementation batches

**The synthesis insight:** The agent identified that fixing alias patterns first would make the file splitting easier later - a dependency that no single analysis agent could have known.

## Step 8: Experience Interactive Mode

Now let's run the analysis in interactive mode to see how we'd actually use this in practice.

```bash
@tech-debt-parallel . --interactive
```

**What happens:**
1. Same parallel analysis runs
2. Synthesis agent presents findings  
3. **We get to choose:** "Proceed with implementation? (y/n)"
4. **We can prioritize:** "Which categories should I focus on? (1,2,3)"

**Try selecting different options** to see how the system responds to our preferences.

## What We've Learned

**True parallel execution:** We experienced 3 agents analyzing simultaneously, demonstrating real concurrency vs. simulation.

**Agent specialization:** Each agent brought focused expertise that produced deeper insights than a general analysis would.

**Intelligent synthesis:** The combination of findings was more valuable than the sum of parts - identifying dependencies and creating actionable plans.

**Interactive control:** We maintained control over what gets implemented while benefiting from AI analysis and planning.

**Performance benefits:** 55% faster analysis with better quality results - this would scale even more dramatically on larger codebases.

## Next Steps

**To continue learning:**
- Try the analysis on your own project
- Experiment with different command options
- Follow the [Building a Custom Sub-Agent Command](./building-custom-command.md) tutorial

**To start using productively:**
- Read [How to Find Technical Debt in Large Codebases](../how-to/find-tech-debt-large-codebases.md)
- Check [How to Prepare for Framework Migrations](../how-to/prepare-framework-migration.md)

**To understand more deeply:**
- Explore [About Parallel Sub-Agent Architecture](../explanation/parallel-architecture.md)
- Study [About Multi-Agent Coordination](../explanation/multi-agent-coordination.md)

## Reflection

We've just experienced a paradigm shift in AI-assisted development. Instead of asking one AI to do everything sequentially, we coordinated multiple specialized AIs working in parallel, then intelligently synthesized their expertise.

This approach scales: imagine 10 agents analyzing a 100,000-file codebase simultaneously, each bringing deep specialization to their domain, with synthesis agents creating comprehensive improvement roadmaps.

**The future of AI development assistance isn't smarter individual agents - it's smarter coordination of specialized agents working together.**

---

*Congratulations! You've experienced true parallel AI execution. Ready to build your own? Try [Building a Custom Sub-Agent Command](./building-custom-command.md).*