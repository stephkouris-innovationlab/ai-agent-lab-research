# AI Agent Lab Research

**Comparing AI Agent Architectures for E-commerce Decision Making**

A systematic study of four AI agent patterns—Single Agent, Multi-Agent, A2A Debate, and MCP-Enhanced—across 60 benchmark runs using Claude Haiku, Sonnet, and Opus models.

[![Live Demo](https://img.shields.io/badge/Live%20Demo-ai--agent--simulator.vercel.app-blue)](https://ai-agent-simulator.vercel.app)
[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc/4.0/)

---

## Executive Summary

### Key Findings

| Finding | Insight |
|---------|---------|
| **Architecture > Model** | Multi-Agent Sonnet outperformed Single Agent Opus on complex tasks |
| **A2A Excels at Trade-offs** | 95.5% confidence on competing priorities vs Single Agent at 38% |
| **Calibration Matters** | Opus knows when it doesn't know (10-18% on impossible tasks vs Sonnet's 95%) |
| **Cost-Confidence Trade-off** | A2A costs 2.6x more but delivers 1.6x higher confidence |

### Quick Numbers

- **60 benchmark runs** across 4 agents × 5 scenarios × 3 models
- **Total API cost:** ~$12
- **Testing period:** January 3-7, 2026

---

## Agent Architectures Tested

| Agent | Description | Avg Time | Avg Cost | Avg Confidence |
|-------|-------------|----------|----------|----------------|
| **Single Agent** | One LLM, one decision | 24s | $0.017 | 55% |
| **Multi-Agent** | Orchestrator + 5 specialists | 32s | $0.028 | 63% |
| **A2A Debate** | Peer agents negotiate consensus | 52s | $0.045 | 86% |
| **MCP-Enhanced*** | Agent with external tools | 1.8s | $0.020 | 84% |

*\*MCP uses simulated tools for demonstration purposes*

---

## When to Use Each Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    DECISION COMPLEXITY                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Simple, clear criteria          → Single Agent                 │
│  (charger under $30)               Fast, cheap, efficient       │
│                                                                 │
│  Complex, multi-dimensional      → Multi-Agent                  │
│  (work headphones)                 Specialist perspectives      │
│                                                                 │
│  Competing priorities            → A2A Debate                   │
│  (budget vs quality vs speed)      Genuine negotiation          │
│                                                                 │
│  Data-dependent                  → MCP-Enhanced                 │
│  (price history, reviews)          External tool access         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Documentation

| Document | Description |
|----------|-------------|
| [Architecture](docs/ARCHITECTURE.md) | Deep dive into each agent pattern |
| [Scenarios](docs/SCENARIOS.md) | The 5 test scenarios and their design rationale |
| [Methodology](docs/METHODOLOGY.md) | Testing protocol, environment, and metrics |
| [Findings](docs/FINDINGS.md) | Complete benchmark results and analysis |
| [Product Catalog](docs/PRODUCT-CATALOG.md) | The 60-product e-commerce dataset |
| [Limitations](docs/LIMITATIONS.md) | Caveats, constraints, and future work |

---

## Research Highlights

### 1. Architecture Encodes Values

On the same "Balanced Choice" scenario, each architecture recommended a **different product**:

| Architecture | Recommendation | Confidence | Why |
|--------------|----------------|------------|-----|
| Single Agent | SkinFirst Toothbrush | 38% | First satisfactory match |
| Multi-Agent | SkinFirst Toothbrush | 65% | Specialist consensus |
| A2A Debate | ElectraMax Speaker | 95% | Negotiated compromise |
| MCP-Enhanced | UrbanStyle Wallet | 98% | Data-driven best value |

**Insight:** The architecture you choose encodes decision-making values. None is "correct"—each reflects a different theory of good decision-making.

### 2. Graceful Failure Detection

When given impossible requirements (premium laptop under $200):

| Agent | Response | Confidence |
|-------|----------|------------|
| Single Agent | Correctly declined | 0% |
| Multi-Agent | Low-confidence fallback | 10% |
| A2A (Sonnet) | Overconfident recommendation | 95% |
| A2A (Opus) | Well-calibrated uncertainty | 10-18% |

**Insight:** For high-stakes applications, model calibration matters more than raw capability.

### 3. Cost-Efficiency Sweet Spots

```
Cost per 1,000 decisions:

Single Agent:  $17  ████████
Multi-Agent:   $28  █████████████
A2A Debate:    $45  █████████████████████
MCP-Enhanced:  $20  █████████
```

A2A's premium is justified for trade-off decisions (2.5x confidence gain) but wasteful for simple purchases.

---

## Try It Yourself

**Live Demo:** [ai-agent-simulator.vercel.app](https://ai-agent-simulator.vercel.app)

The interactive demo lets you:
- Run simulations with any agent type
- Watch real-time reasoning via SSE streaming
- Compare results across scenarios
- View telemetry (tokens, cost, latency)

*Demo mode uses pre-recorded responses. Real API mode available with your own Claude API key.*

---

## Data Files

| File | Description |
|------|-------------|
| [benchmark-results.json](data/benchmark-results.json) | Raw benchmark data from 60 runs |
| [scenarios.json](data/scenarios.json) | Scenario definitions |

---

## Citation

If you use this research in your work:

```
@misc{kouris2026aiagentlab,
  author = {Kouris, Stephan},
  title = {AI Agent Lab: Comparing Agent Architectures for E-commerce Decision Making},
  year = {2026},
  url = {https://github.com/stephkouris-innovationlab/ai-agent-lab-research}
}
```

---

## License

This research is licensed under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/).

You are free to share and adapt this material for non-commercial purposes with attribution.

---

## Author

**Stephan Kouris**
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://www.linkedin.com/in/stephankouris/)

---

*For educational purposes only. AI outputs are simulated demonstrations of agent architectures.*
