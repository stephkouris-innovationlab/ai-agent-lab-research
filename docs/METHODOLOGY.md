# Research Methodology

This document describes how we conducted benchmark testing, the environment configuration, metrics collected, and the testing protocol.

---

## Test Matrix

| Dimension | Values | Count |
|-----------|--------|-------|
| Agent Types | Single, Multi, A2A, MCP | 4 |
| Scenarios | Quick Purchase, Informed Decision, Balanced Choice, Data-Driven, Edge Case Testing | 5 |
| Models | Haiku 4.5, Sonnet 4.5, Opus 4.5 | 3 |
| **Total Runs** | 4 × 5 × 3 | **60** |

### Primary vs Calibration Scenarios

We distinguish between two types of scenarios for metric calculations:

| Category | Scenarios | Purpose | Metric Usage |
|----------|-----------|---------|--------------|
| **Primary (4)** | Quick Purchase, Informed Decision, Balanced Choice, Data-Driven | Test decision-making quality | Used for "Top Performer" and confidence averages |
| **Calibration (1)** | Edge Case Testing | Test confidence accuracy on impossible tasks | Excluded from performance averages |

**Why the distinction?** The Edge Case Testing scenario has intentionally contradictory requirements. High confidence there indicates **overconfidence** (failure), not good performance. Including it in averages would artificially inflate A2A's scores (95% on impossible = bad, not good).

### Summary Statistics

| Metric | Value |
|--------|-------|
| Total Benchmark Runs | 60 |
| Iterations per Combination | 1 |
| Total API Cost | ~$12 |
| Total Execution Time | ~55 minutes |
| Testing Period | January 3-7, 2026 |

---

## Testing Protocol

### Step 1: Environment Setup (~5 min)

- Configure API keys and validate connectivity
- Initialize logging and telemetry collection
- Verify database connections

### Step 2: Test Matrix Generation (~1 min)

- Generate all 60 combinations (4 agents × 5 scenarios × 3 models)
- Randomize execution order to reduce ordering bias

### Step 3: Benchmark Execution (~45 min)

- Execute each combination via SSE streaming
- Capture all telemetry in real-time
- Log raw API responses for analysis

### Step 4: Data Collection (Real-time)

Metrics captured for each run:
- Response time (ms)
- Token counts (input + output)
- Cost calculations (based on Claude pricing)
- Confidence scores (agent self-reported)
- Product recommendations

### Step 5: Statistical Analysis (~3 min)

- Compute averages, variance, and correlations
- Identify outliers
- Validate consistency across model comparisons

### Step 6: Quality Validation (~5 min)

- Manual review of edge cases
- Verify impossible task responses
- Check calibration accuracy on low-confidence results

---

## Testing Environment

### API Configuration

| Setting | Value |
|---------|-------|
| Claude Models | claude-sonnet-4-5 (primary benchmark model) |
| API Version | 2024-01 (Anthropic SDK) |
| Max Tokens | 4096 per request |
| Temperature | 0.7 (balanced creativity) |

### Test Infrastructure

| Component | Technology |
|-----------|------------|
| Runtime | Node.js 20 LTS |
| Framework | Express.js + TypeScript |
| Database | SQLite (via Prisma) |
| Logging | Pino (structured JSON) |

### Execution Context

| Setting | Value |
|---------|-------|
| Date Range | January 3-7, 2026 |
| Concurrency | Sequential (no parallelism) |
| Rate Limiting | 1 request per 2 seconds |
| Timeout | 120 seconds per request |

---

## Metrics Collected

### 1. Response Time

| Metric | Details |
|--------|---------|
| Unit | Seconds |
| Measurement | Total time from request to final recommendation |
| Importance | Measures user experience and practical usability |

### 2. Token Usage

| Metric | Details |
|--------|---------|
| Unit | Tokens |
| Measurement | Total input + output tokens consumed |
| Importance | Directly correlates with API costs and reasoning depth |

### 3. Estimated Cost

| Metric | Details |
|--------|---------|
| Unit | USD |
| Measurement | Calculated cost based on Claude API pricing |
| Importance | Critical for production deployment planning |

### 4. Confidence Score

| Metric | Details |
|--------|---------|
| Unit | Percentage (0-100%) |
| Measurement | Agent self-reported confidence in recommendation |
| Importance | Quality signal—low confidence indicates uncertainty |

---

## Model Pricing (January 2026)

| Model | Input (per 1M tokens) | Output (per 1M tokens) |
|-------|----------------------|------------------------|
| Haiku 4.5 | $0.80 | $4.00 |
| Sonnet 4.5 | $3.00 | $15.00 |
| Opus 4.5 | $15.00 | $75.00 |

---

## Product Catalog

Our test catalog contains 60 curated products designed to enable meaningful agent comparisons.

| Category | Count | Examples |
|----------|-------|----------|
| Electronics | 9 | Chargers, headphones, speakers, webcams |
| Home Appliances | 7 | Coffee makers, vacuums, air purifiers |
| Fashion | 7 | T-shirts, jeans, wallets, boots |
| Books & Media | 6 | Business books, tech guides, courses |
| Groceries | 7 | Coffee, snacks, supplements |
| Furniture | 7 | Office chairs, desks, sofas |
| Sports & Outdoors | 7 | Yoga mats, dumbbells, camping gear |
| Beauty | 7 | Skincare, haircare, oral care |

**Price Range:** $15 - $350
**Average Rating:** 4.2 stars

See [PRODUCT-CATALOG.md](PRODUCT-CATALOG.md) for full details.

---

## Data Recording Process

### Recording Format

Each benchmark run produces:

```json
{
  "id": "run-001",
  "timestamp": "2026-01-03T10:23:45Z",
  "agent": "single",
  "scenario": "quick-purchase",
  "model": "claude-sonnet-4-5-20250929",
  "metrics": {
    "timeMs": 20949,
    "tokens": 2612,
    "costUsd": 0.016,
    "confidence": 80.5
  },
  "result": {
    "productId": "electronics-3",
    "productName": "NovaTech USB-C Fast Charger",
    "reasoning": "..."
  }
}
```

### SSE Streaming

All agent reasoning is captured via Server-Sent Events:

```
data: {"type":"thinking","content":"Analyzing requirements..."}
data: {"type":"thinking","content":"Filtering products..."}
data: {"type":"result","confidence":80.5,"product":"..."}
```

---

## Statistical Approach

### Aggregation Method

1. **Per-agent averages**: Average across all scenarios and models for each agent type
2. **Per-scenario breakdown**: Compare agents on specific scenarios
3. **Per-model comparison**: Compare Haiku vs Sonnet vs Opus

### Outlier Handling

- Impossible task results analyzed separately (0% confidence is correct behavior, not an error)
- All 60 benchmark runs completed successfully

### Confidence Intervals

With N=1 per combination, we focus on:
- Directional differences (Agent A > Agent B)
- Pattern consistency (does the pattern hold across scenarios?)
- Qualitative analysis of reasoning traces

---

## Reproducibility

### What's Reproducible

- Product catalog (seeded random generation, deterministic)
- Scenario definitions (fixed requirements)
- Agent architectures (code available)
- Prompt templates (standardized)

### What's Not Reproducible

- Exact LLM outputs (non-deterministic even with same temperature)
- API latency (varies with server load)
- Model version behavior (models may be updated)

### Reproduction Instructions

1. Clone the repository
2. Set `ANTHROPIC_API_KEY` environment variable
3. Run `npm run benchmark`
4. Results saved to `data/benchmark-results.json`

---

## Ethical Considerations

### No Real Transactions

All recommendations are simulated. No actual purchases were made.

### No PII

Product data is synthetic. No real customer data was used.

### API Usage

Total API cost (~$12) was paid by the researcher. No free tier abuse.

### Open Research

Methodology and data are published for reproducibility and peer review.
