# Agent Architectures

This document provides a deep dive into the four AI agent patterns tested in this research.

---

## Overview

| Architecture | Pattern | API Calls | Best For |
|--------------|---------|-----------|----------|
| Single Agent | Augmented LLM | 1 | Simple, clear decisions |
| Multi-Agent | Orchestrator-Workers | 6 | Complex, multi-dimensional analysis |
| A2A Debate | Multi-Agent Debate | 8-12 | Trade-off heavy decisions |
| MCP-Enhanced | Tool Use | 1-3 | Data-driven decisions |

---

## 1. Single Agent

### Description

One LLM handles the entire task from start to finish. The simplest and most direct approach.

```
┌─────────────────────────────────────────┐
│              Single Agent               │
│  ┌─────────────────────────────────┐    │
│  │         Claude API Call         │    │
│  │  • Analyze requirements         │    │
│  │  • Evaluate all products        │    │
│  │  • Make recommendation          │    │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

### When to Use

- Simple, well-defined tasks
- Clear criteria with few trade-offs
- When speed and cost efficiency matter most
- Tasks that don't require specialized expertise

### Strengths

- **Fastest execution** (24s average)
- **Lowest cost** ($0.017 per decision)
- Simplest to understand and debug
- No coordination overhead
- Best at recognizing impossible tasks (returns null with 0% confidence)

### Weaknesses

- May miss nuances in complex decisions
- Single perspective can lead to bias
- Limited by context window
- Lower confidence on trade-off scenarios (38% on Balanced Choice)

### Benchmark Results

| Metric | Value |
|--------|-------|
| Avg Time | 23.6s |
| Avg Tokens | 2,365 |
| Avg Cost | $0.017 |
| Avg Confidence | 55% |

---

## 2. Multi-Agent (Orchestrator-Workers)

### Description

An orchestrator agent delegates tasks to specialized worker agents, then synthesizes their findings into a final decision.

```
┌─────────────────────────────────────────────────────────────┐
│                    Orchestrator                             │
│  ┌─────────────────────────────────────────────────────┐    │
│  │  "Analyze headphones for work-from-home use"        │    │
│  └─────────────────────────────────────────────────────┘    │
│           │                                                 │
│           ▼                                                 │
│  ┌────────┬────────┬────────┬────────┬────────┐            │
│  │ Price  │Quality │  User  │  Risk  │Product │            │
│  │Analyst │ Expert │Matcher │Assessor│Research│            │
│  └────────┴────────┴────────┴────────┴────────┘            │
│           │                                                 │
│           ▼                                                 │
│  ┌─────────────────────────────────────────────────────┐    │
│  │        Synthesize → Final Recommendation            │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Specialists

| Specialist | Focus |
|------------|-------|
| **Price Analyst** | Value-per-dollar, price history, deals |
| **Quality Expert** | Build quality, durability, warranty |
| **User Matcher** | Fit for user's specific needs |
| **Risk Assessor** | Return policies, seller reliability |
| **Product Researcher** | Specs, features, comparisons |

### When to Use

- Complex tasks with multiple dimensions
- When specialized expertise is valuable
- Tasks requiring parallel analysis
- Decisions needing comprehensive evaluation

### Strengths

- Specialized agents for each aspect
- More thorough analysis
- **89.7% confidence on complex tasks** (Informed Decision)
- Each specialist goes deep on their domain

### Weaknesses

- Higher cost ($0.028 per decision)
- Slower than single agent (32s average)
- Orchestrator may miss specialist nuances
- Coordination overhead

### Benchmark Results

| Metric | Value |
|--------|-------|
| Avg Time | 31.9s |
| Avg Tokens | 3,350 |
| Avg Cost | $0.028 |
| Avg Confidence | 63% |

### Key Insight

Specialists operate in "expert mode" vs Single Agent's "checklist mode." The Price Analyst doesn't just check if something is affordable—it calculates value-per-dollar, compares to category averages, and flags outliers.

---

## 3. A2A Debate (Agent-to-Agent)

### Description

Multiple peer agents with different priorities debate and negotiate to reach consensus. Each agent advocates for their specialty.

```
┌─────────────────────────────────────────────────────────────┐
│                      A2A Debate                             │
│                                                             │
│  Round 1: Opening Statements                                │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │  Budget  │  │ Quality  │  │  Speed   │  │Sustainab.│    │
│  │ Advocate │  │ Advocate │  │ Advocate │  │ Advocate │    │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘    │
│       │              │              │              │        │
│       └──────────────┴──────────────┴──────────────┘        │
│                          │                                  │
│                          ▼                                  │
│  Round 2: Rebuttals & Counter-arguments                     │
│                          │                                  │
│                          ▼                                  │
│  Round 3: Consensus Building                                │
│                          │                                  │
│                          ▼                                  │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              Final Consensus Decision               │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Peer Agents

| Agent | Priority | Argues For |
|-------|----------|------------|
| **Budget Advocate** | Cost | Lowest price, value for money |
| **Quality Advocate** | Quality | Best build, durability |
| **Speed Advocate** | Time | Fastest shipping, availability |
| **Sustainability Advocate** | Eco | Environmental impact, certifications |

### When to Use

- Conflicting priorities need balancing
- Decisions with multiple stakeholders
- When trade-offs are subjective
- Complex negotiations between concerns

### Strengths

- **95.5% confidence on trade-off scenarios** (Balanced Choice)
- Surfaces hidden trade-offs through debate
- Balanced decisions across priorities
- Transparent reasoning process
- Genuine negotiation (agents change positions!)

### Weaknesses

- Slowest architecture (52s average)
- Highest cost ($0.045 per decision)
- May not reach consensus
- **Overconfident on impossible tasks** (95% with Sonnet)

### Benchmark Results

| Metric | Value |
|--------|-------|
| Avg Time | 52.2s |
| Avg Tokens | 5,278 |
| Avg Cost | $0.045 |
| Avg Confidence | 86% |

### Key Insight

In our testing, A2A agents **genuinely negotiated**—not just role-played. The Budget Advocate changed position after hearing Quality's "cost-per-use" framing. The final consensus often reflected compromise positions that weren't in any agent's opening statement.

---

## 4. MCP-Enhanced (Tool Use)

### Description

Agent augmented with external tools via Model Context Protocol. Can query databases, APIs, and external services for real data.

```
┌─────────────────────────────────────────────────────────────┐
│                    MCP-Enhanced Agent                       │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                    Claude API                        │    │
│  └─────────────────────────────────────────────────────┘    │
│           │                                                 │
│           ▼                                                 │
│  ┌────────────────────────────────────────────────────┐     │
│  │               Tool Invocations                      │     │
│  │  ┌──────────────┐ ┌──────────────┐ ┌────────────┐  │     │
│  │  │searchProducts│ │getPriceHist. │ │ getReviews │  │     │
│  │  └──────────────┘ └──────────────┘ └────────────┘  │     │
│  └────────────────────────────────────────────────────┘     │
│           │                                                 │
│           ▼                                                 │
│  ┌─────────────────────────────────────────────────────┐    │
│  │         Data-Driven Recommendation                  │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

### Available Tools

| Tool | Purpose |
|------|---------|
| `searchProducts()` | Query product catalog with filters |
| `getPriceHistory()` | Get historical price data |
| `getReviews()` | Fetch and analyze review sentiment |

### When to Use

- Decisions requiring external data
- When real-time information is valuable
- Price comparisons, trend analysis
- Data-driven recommendations

### Strengths

- **Fastest response time** (1.8s average)*
- Access to external data sources
- Data-driven decisions vs pure reasoning
- Can verify facts and claims

### Weaknesses

- *Our implementation uses simulated tools
- Depends on tool availability/quality
- Tool calls add latency in real deployments
- More complex to set up

### Benchmark Results

| Metric | Value |
|--------|-------|
| Avg Time | 1.77s* |
| Avg Tokens | 3,700 |
| Avg Cost | $0.020 |
| Avg Confidence | 84% |

*\*Times reflect simulated tool responses, not production MCP performance*

### Important Note

Our MCP implementation demonstrates the **pattern** but uses simulated tools rather than real Claude API tool-use. Results show architectural potential but not production benchmark data. Real MCP integration would enable live price checks, inventory validation, and review aggregation.

---

## Architecture Comparison Matrix

| Dimension | Single | Multi | A2A | MCP* |
|-----------|--------|-------|-----|------|
| Speed | Fast | Medium | Slow | Fastest |
| Cost | Low | Medium | High | Low |
| Confidence (avg) | 55% | 63% | 86% | 84% |
| Trade-off Handling | Poor | Medium | Excellent | Good |
| Impossible Task | Correct | Appropriate | Overconfident | Uncertain |
| Complexity | Simple | Medium | High | Medium |

---

## Choosing an Architecture

```
                    ┌─────────────────────┐
                    │  What type of       │
                    │  decision?          │
                    └─────────────────────┘
                              │
            ┌─────────────────┼─────────────────┐
            │                 │                 │
            ▼                 ▼                 ▼
    ┌───────────────┐ ┌───────────────┐ ┌───────────────┐
    │ Simple,       │ │ Complex,      │ │ Competing     │
    │ clear criteria│ │ multi-factor  │ │ priorities    │
    └───────────────┘ └───────────────┘ └───────────────┘
            │                 │                 │
            ▼                 ▼                 ▼
    ┌───────────────┐ ┌───────────────┐ ┌───────────────┐
    │ Single Agent  │ │ Multi-Agent   │ │ A2A Debate    │
    └───────────────┘ └───────────────┘ └───────────────┘


                    ┌─────────────────────┐
                    │  Need external      │
                    │  data?              │
                    └─────────────────────┘
                              │
                              ▼
                    ┌───────────────┐
                    │ MCP-Enhanced  │
                    └───────────────┘
```

---

## References

- [Anthropic: Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents)
- [Model Context Protocol](https://modelcontextprotocol.io/)
