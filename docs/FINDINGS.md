# Research Findings

Complete benchmark results and analysis from 60 runs across 4 agent architectures, 5 scenarios, and 3 Claude models.

---

## Executive Summary

### Top-Level Results (Primary Scenarios)

| Agent | Avg Time | Avg Cost | Avg Confidence† | Best Scenario |
|-------|----------|----------|-----------------|---------------|
| Single | 27s | $0.022 | 69% | Quick Purchase |
| Multi | 35s | $0.031 | 76% | Informed Decision |
| A2A | 52s | $0.045 | 84% | Balanced Choice |
| MCP* | 1.8s | $0.020 | 93% | Data-Driven |

*MCP uses simulated tools

†**Note:** Averages are from **4 primary scenarios only**. The 5th scenario (Edge Case Testing) tests calibration, not performance—A2A's 95% confidence there is a **failure mode**, not success. Including it would artificially inflate A2A's average.

---

## Key Insights

### 1. A2A Excels at Trade-offs

**Finding:** A2A Debate achieves 95.5% confidence on Balanced Choice vs Single Agent at 38%.

When priorities compete, the debate pattern genuinely negotiates trade-offs. This validates A2A for gift purchases and team decisions.

```
Balanced Choice Confidence:
Single  ███████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  38%
Multi   ████████████████████████████████░░░░░░░░░░░░░░░░░░  65%
A2A     ███████████████████████████████████████████████░░░  95.5%
MCP     █████████████████████████████████████████████████░  98%
```

### 2. Cost-Efficiency Trade-off

**Finding:** Architecture choice impacts costs significantly.

| Architecture | Cost per Decision | Cost per 1,000 | Confidence |
|--------------|-------------------|----------------|------------|
| Single Agent | $0.017 | $17 | 55% |
| Multi-Agent | $0.028 | $28 | 63% |
| A2A Debate | $0.045 | $45 | 86% |
| MCP-Enhanced | $0.020 | $20 | 84% |

A2A delivers 1.6x higher confidence for 2.6x the cost.

### 3. Multi-Agent Wins Complex Tasks

**Finding:** Multi-Agent achieves 89.7% confidence on Informed Decision vs Single Agent at 77.9%.

Specialist perspectives justify the 35.8s vs 25.8s time trade-off for complex analysis scenarios.

### 4. Architecture Shapes Recommendations

**Finding:** Same requirements, different answers.

On Balanced Choice:
| Agent | Product | Confidence |
|-------|---------|------------|
| Single | SkinFirst Toothbrush | 38% |
| Multi | SkinFirst Toothbrush | 65% |
| A2A | ElectraMax Speaker | 95.5% |
| MCP | UrbanStyle Wallet | 98% |

**Insight:** Each architecture encodes different values. Choose based on decision type.

### 5. Tool-Use Shows Promise*

**Finding:** MCP demonstrates data-driven potential with 84% average confidence.

*Our implementation uses simulated tools. Real MCP integration would enable live data access.

### 6. Edge Case Calibration (Critical Finding)

**Finding:** A2A Sonnet shows **dangerous overconfidence** (95%) on impossible tasks—this is a FAILURE MODE, not impressive performance.

| Agent | Edge Case Response | Confidence | Assessment |
|-------|-------------------|------------|------------|
| Single | Correctly declined | 0% | ✅ CORRECT |
| Multi | Low-confidence fallback | 10% | ✅ CORRECT |
| A2A Sonnet | Overconfident recommendation | 95% | ❌ FAILURE |
| A2A Opus | Well-calibrated uncertainty | 10-18% | ✅ CORRECT |
| MCP | Moderate uncertainty | 47.5% | ⚠️ Moderate |

> **Why This Matters:** A2A's high average confidence across all scenarios is **inflated by this failure mode**. When we exclude Edge Case Testing from averages (as we should for performance metrics), MCP emerges as the top performer, not A2A. The 95% confidence on impossible requirements is not impressive—it's dangerous for production systems.

### 7. Model Calibration Varies

**Finding:** Opus shows best calibration on edge cases.

On impossible tasks:
- Opus A2A: 10-18% confidence (well-calibrated)
- Sonnet A2A: 95% confidence (overconfident)

Opus costs 5x more but knows when it doesn't know.

---

## Benchmark Data

### Agent Benchmarks (Aggregated)

| Agent | Avg Time (ms) | Avg Tokens | Avg Cost ($) | Avg Confidence | Sample Size |
|-------|---------------|------------|--------------|----------------|-------------|
| Single | 23,604 | 2,365 | 0.017 | 55.0% | 15 |
| Multi | 31,886 | 3,350 | 0.028 | 63.1% | 15 |
| A2A | 52,174 | 5,278 | 0.045 | 85.8% | 15 |
| MCP | 1,770 | 3,700 | 0.020 | 83.9% | 15 |

### Scenario Benchmarks

#### Quick Purchase

| Agent | Time (ms) | Tokens | Cost ($) | Confidence | Product |
|-------|-----------|--------|----------|------------|---------|
| Single | 20,949 | 2,612 | 0.016 | 80.5% | NovaTech USB-C Fast Charger |
| Multi | 25,599 | 3,581 | 0.025 | 67.5% | NovaTech USB-C Fast Charger |
| A2A | 51,825 | 5,341 | 0.047 | 77.4% | NovaTech USB-C Fast Charger |
| MCP | 1,260 | 3,700 | 0.020 | 85.5% | NovaTech USB-C Fast Charger |

**Best Agent:** MCP (highest confidence, fastest time)

#### Informed Decision

| Agent | Time (ms) | Tokens | Cost ($) | Confidence | Product |
|-------|-----------|--------|----------|------------|---------|
| Single | 25,775 | 3,149 | 0.023 | 77.9% | DigitalEdge Headset Pro |
| Multi | 35,833 | 4,042 | 0.032 | 89.7% | DigitalEdge Headset Pro |
| A2A | 47,603 | 5,022 | 0.042 | 86.9% | DigitalEdge Headphones |
| MCP | 1,861 | 3,700 | 0.020 | 90.3% | DigitalEdge Headphones |

**Best Agent:** MCP (90.3% confidence)

#### Balanced Choice

| Agent | Time (ms) | Tokens | Cost ($) | Confidence | Product |
|-------|-----------|--------|----------|------------|---------|
| Single | 30,899 | 2,303 | 0.023 | 38.3% | SkinFirst Electric Toothbrush |
| Multi | 38,271 | 3,029 | 0.029 | 64.6% | SkinFirst Electric Toothbrush |
| A2A | 57,260 | 5,426 | 0.047 | 95.5% | ElectraMax Portable Speaker |
| MCP | 2,011 | 3,700 | 0.020 | 98.0% | UrbanStyle Leather Wallet |

**Best Agent:** MCP (98% confidence, but A2A shows best negotiation)

#### Data-Driven

| Agent | Time (ms) | Tokens | Cost ($) | Confidence | Product |
|-------|-----------|--------|----------|------------|---------|
| Single | 28,541 | 3,759 | 0.025 | 78.2% | NovaTech USB-C Fast Charger |
| Multi | 40,903 | 4,419 | 0.037 | 83.6% | NovaTech USB-C Fast Charger |
| A2A | 51,115 | 5,264 | 0.045 | 75.1% | NovaTech USB-C Fast Charger |
| MCP | 1,861 | 3,700 | 0.020 | 98.0% | NovaTech USB-C Fast Charger |

**Best Agent:** MCP (designed for data-driven decisions)

#### Edge Case Testing (Calibration Scenario)

> ⚠️ **This scenario tests calibration, not performance. Lower confidence is BETTER here.**

| Agent | Time (ms) | Tokens | Cost ($) | Confidence | Assessment |
|-------|-----------|--------|----------|------------|------------|
| Single | 11,854 | 0 | 0.000 | 0% | ✅ CORRECT |
| Multi | 18,823 | 1,678 | 0.015 | 10% | ✅ CORRECT |
| A2A | 48,497 | 5,080 | 0.043 | 95% | ❌ FAILURE |
| MCP | 1,857 | 3,700 | 0.020 | 47.5% | ⚠️ Moderate |

**Best Agent:** Single (correctly recognized impossibility with 0% confidence)

**Critical Note:** A2A's 95% confidence here is a **dangerous failure mode**—it recommended an "Unknown" product that didn't exist. This result is excluded from primary scenario averages because it tests calibration, not decision-making quality.

---

## Model Benchmarks

| Model | Avg Time (ms) | Avg Cost ($) | Avg Confidence | Cost per 1,000 | Best For |
|-------|---------------|--------------|----------------|----------------|----------|
| Haiku 4.5 | 20,400 | 0.010 | 77% | $10 | High-volume, cost-sensitive |
| Sonnet 4.5 | 35,400 | 0.031 | 71% | $31 | Balanced performance |
| Opus 4.5 | 36,000 | 0.168 | 64% | $168 | Best calibration on edge cases |

### Surprising Finding: Haiku Outperforms

Haiku at $0.010/request achieved 77% average confidence—higher than Sonnet's 71%. For straightforward decisions, smaller models may actually be better (less overthinking).

---

## Performance Radar

Normalized scores (0-100) across five dimensions:

| Agent | Speed | Cost Efficiency | Confidence | Reliability | Simplicity |
|-------|-------|-----------------|------------|-------------|------------|
| Single | 58 | 100 | 55 | 95 | 100 |
| Multi | 43 | 61 | 63 | 85 | 50 |
| A2A | 35 | 38 | 86 | 45 | 40 |
| MCP | 100 | 85 | 84 | 70 | 70 |

### Scoring Methodology

- **Speed:** Inverse of avg time (MCP 1.8s=100, A2A 52.2s=35)
- **Cost Efficiency:** Inverse of avg cost ($0.017=100, $0.045=38)
- **Confidence:** Direct from benchmark averages
- **Reliability:** Based on impossible task handling
- **Simplicity:** Architecture complexity (single call=100, debate rounds=40)

---

## Architecture Recommendations

### Single Agent

**When to Use:**
- Quick purchases under $50
- Clear, non-conflicting requirements
- High-volume, cost-sensitive operations
- Graceful failure detection needed

**Strengths:** Fast (24s), Low cost ($0.017), Best at declining impossible tasks

**Weaknesses:** Lower confidence (55%), Struggles with trade-offs (38%)

### Multi-Agent

**When to Use:**
- Important purchases ($100+)
- Complex analysis needed
- Multiple evaluation criteria
- Enterprise applications

**Strengths:** Specialist perspectives, 89.7% on complex tasks

**Weaknesses:** Slower (32s), Higher cost ($0.028)

### A2A Debate

**When to Use:**
- Competing priorities
- Gift purchases with trade-offs
- Team decision simulation
- Explicit negotiation needed

**Strengths:** 95.5% on trade-offs, Genuine consensus building

**Weaknesses:** Slowest (52s), Highest cost ($0.045), Overconfident on impossible

### MCP-Enhanced

**When to Use:**
- Price-sensitive shopping
- Real-time data integration
- Inventory-dependent decisions
- Data-driven analysis

**Strengths:** Fastest (1.8s), Tool-use pattern, 84% confidence

**Weaknesses:** Simulated tools in demo, Requires real MCP setup

---

## Practitioner Takeaways

### 1. Choose Architecture Before Model

Multi-Agent Sonnet outperformed Single Agent Opus on complex tasks. Architecture often matters more than raw model capability.

**Recommendation:** Design your agent system first, then select the model.

### 2. Match Architecture to Decision Complexity

A2A adds 2.4x latency but 2x confidence on trade-offs. Simple decisions don't benefit from complex architectures.

**Recommendation:** Implement a router that classifies decision complexity.

### 3. Test Calibration on Edge Cases

Sonnet A2A was dangerously overconfident (95%) on impossible tasks.

**Recommendation:** Always include impossible/adversarial scenarios in testing.

### 4. Use Confidence as Routing Signal

Low confidence (<50%) consistently indicated uncertain decisions.

**Recommendation:** Route low-confidence results to human review.

### 5. Expose Reasoning for User Trust

Users trusted recommendations more when watching the reasoning process.

**Recommendation:** Show "the work" even if it adds latency.
