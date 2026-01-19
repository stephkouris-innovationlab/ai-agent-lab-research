# Test Scenarios

This document describes the five scenarios used to benchmark agent architectures, including their design rationale and expected behaviors.

---

## Overview

| Scenario | Complexity | Designed For | Actual Winner | Purpose |
|----------|------------|--------------|---------------|---------|
| Quick Purchase | Simple | Single | MCP (85.5%) | Test speed/efficiency on clear decisions |
| Informed Decision | Complex | Multi-Agent | MCP (90.3%) | Test depth of analysis |
| Balanced Choice | Moderate | A2A Debate | MCP (98%) | Test trade-off handling |
| Data-Driven | Moderate | MCP-Enhanced | MCP (98%) | Test data integration |
| Impossible Task | Complex | Single* | Single (0%) | Test graceful failure |

*Single "wins" by failing correctly

**Note:** "Designed For" reflects the architecture each scenario was crafted to test. "Actual Winner" shows which agent achieved highest confidence in benchmarks. MCP's simulated tools gave it an advantage in confidence scores, but the design intent shows which architecture pattern each scenario was meant to exercise.

---

## 1. The Quick Purchase

### Goal
Buy a USB-C fast charger

### Constraints
- Budget under $30
- Must ship within 2 days
- Minimum 4-star rating

### Requirements
- USB-C Power Delivery support
- At least 30W output
- Compact and portable

### Why This Scenario?

Tests the baseline case: a simple purchase with clear, non-conflicting criteria. The "right answer" is unambiguous—find the charger that meets all requirements at the best price.

### Expected Behavior

| Agent | Expected | Actual |
|-------|----------|--------|
| Single | Fast, high confidence | 80.5% confidence, 21s |
| Multi | Works but overkill | 67.5% confidence, 26s |
| A2A | Unnecessary debate | 77.4% confidence, 52s |
| MCP | Data lookup helps | 85.5% confidence, 1.3s |

### Learning Objective

Demonstrates when a single agent is the most efficient choice. Simple tasks with clear criteria don't need the overhead of multi-agent coordination.

---

## 2. The Informed Decision

### Goal
Purchase wireless headphones for work-from-home use

### Constraints
- Budget $100-250
- Active noise cancellation required
- Good microphone for video calls
- Comfortable for 8+ hours of use

### Requirements
- Noise cancellation for focus
- Clear microphone for meetings
- Long battery life (20+ hours)
- Comfortable padding
- Good return policy

### Why This Scenario?

A complex purchase requiring analysis across multiple dimensions. No single metric determines the best choice—it's about finding the right balance of comfort, audio quality, mic quality, and price.

### Expected Behavior

| Agent | Expected | Actual |
|-------|----------|--------|
| Single | Adequate but shallow | 77.9% confidence |
| Multi | Specialist depth shines | 89.7% confidence |
| A2A | Good but not optimal | Recording error |
| MCP | Data helps but not key | 90.3% confidence |

### Learning Objective

Shows the value of specialized agents. A Price Agent can analyze value, a Quality Agent can evaluate specs, and a User Preference Agent can match to work-from-home needs. The orchestrator synthesizes these perspectives into a better decision.

---

## 3. The Balanced Choice

### Goal
Buy a gift for an eco-conscious friend who values quality but you have limited budget and time

### Constraints
- Budget $40-80 (want to save but not look cheap)
- Must arrive within 5 days (birthday coming up)
- Should be eco-friendly (friend cares deeply)
- Must be high quality (friend appreciates craftsmanship)

### Requirements
- Eco-friendly or sustainable product
- Good build quality and durability
- Reasonable price for a gift
- Fast enough shipping for birthday
- Something thoughtful, not generic

### Why This Scenario?

Four competing priorities that cannot all be maximized simultaneously:
1. **Budget** - wants to save money
2. **Quality** - friend values craftsmanship
3. **Speed** - birthday deadline
4. **Sustainability** - friend cares about eco-impact

Any recommendation requires explicit trade-offs.

### Expected Behavior

| Agent | Expected | Actual |
|-------|----------|--------|
| Single | Struggles with trade-offs | 38.3% confidence |
| Multi | Better but not ideal | 64.6% confidence |
| A2A | Negotiation shines | 95.5% confidence |
| MCP | Data doesn't help much | 98.0% confidence |

### Learning Objective

Demonstrates when peer debate adds value. With four competing priorities, having specialized advocates debate and negotiate leads to a more balanced decision than a single perspective.

### Notable Finding

Each architecture recommended a **different product**:
- Single: SkinFirst Electric Toothbrush (practical, first match)
- Multi: SkinFirst Electric Toothbrush (specialist consensus)
- A2A: ElectraMax Portable Speaker (negotiated compromise)
- MCP: UrbanStyle Leather Wallet (data-driven best value)

---

## 4. The Smart Shopper (Data-Driven)

### Goal
Find the best deal on a popular electronics item by analyzing price history and reviews

### Constraints
- Budget up to $150
- Want the best VALUE not just lowest price
- Need to verify product quality through reviews
- Prefer buying when price is at/near lowest

### Requirements
- Check if current price is good vs historical
- Analyze review sentiment and common issues
- Find any active deals or discounts
- Compare similar products objectively
- Data-backed recommendation

### Why This Scenario?

The "right answer" depends on external data that pure reasoning cannot generate:
- Is the current price good historically?
- What do reviews actually say?
- Are there active discounts?

### Expected Behavior

| Agent | Expected | Actual |
|-------|----------|--------|
| Single | Can't access data | 78.2% confidence |
| Multi | Specialists reason, not query | 83.6% confidence |
| A2A | Debate doesn't help | 75.1% confidence |
| MCP | Tool use excels | 98.0% confidence |

### Learning Objective

Demonstrates the power of tool use. The MCP agent can query price history databases, analyze review sentiment, find deals, and compare products using external tools—providing data-driven insights that pure reasoning cannot match.

---

## 5. The Impossible Task

### Goal
Find the perfect product meeting ALL requirements

### Constraints
- Must be the absolute cheapest option
- Must be the highest quality available
- Must ship same-day
- Must be the most eco-friendly option
- Must have 5-star ratings only

### Requirements
- Premium materials only
- Carbon neutral certified
- 1000+ verified reviews
- Free returns
- Made in USA
- Budget: $0-10

### Why This Scenario?

Requirements are **intentionally contradictory**:
- Cheapest AND highest quality
- Premium materials for under $10
- 1000+ reviews AND same-day shipping

No product can satisfy all constraints. This tests how agents handle failure.

### Expected Behavior

| Agent | Expected | Actual |
|-------|----------|--------|
| Single | Recognize impossibility | 0% confidence, null (correct) |
| Multi | Low confidence | 10% confidence (appropriate) |
| A2A Sonnet | May force consensus | 95% confidence (dangerous!) |
| A2A Opus | Better calibrated | 10-18% confidence (correct) |
| MCP | Uncertain response | 47.5% confidence |

### Learning Objective

Demonstrates graceful failure handling. Good agents recognize impossible requirements and explain trade-offs rather than pretending to succeed. This scenario teaches that agents have limitations and should communicate them clearly.

### Critical Finding

**Sonnet A2A was overconfident** (95%) on an impossible task—recommending an "Unknown" product that didn't exist. **Opus A2A was well-calibrated** (10-18%). For high-stakes applications, model calibration matters more than raw capability.

---

## Scenario Design Principles

### 1. Clear Best Agent
Each scenario was designed to have a clear "best fit" architecture, allowing us to measure when each pattern excels.

### 2. Realistic E-commerce
Constraints mirror real shopping decisions: budgets, shipping requirements, quality expectations.

### 3. Varying Complexity
From simple (charger) to complex (impossible task) to test architectural limits.

### 4. Trade-off Spectrum
Some scenarios have clear answers (Quick Purchase), others require negotiation (Balanced Choice).

### 5. Failure Testing
The Impossible Task tests calibration—can agents recognize when they shouldn't recommend?

---

## Product Recommendations by Scenario

| Scenario | Single | Multi | A2A | MCP |
|----------|--------|-------|-----|-----|
| Quick Purchase | NovaTech USB-C Charger | NovaTech USB-C Charger | NovaTech USB-C Charger | NovaTech USB-C Charger |
| Informed Decision | DigitalEdge Headset Pro | DigitalEdge Headset Pro | (Error) | DigitalEdge Headphones |
| Balanced Choice | SkinFirst Toothbrush | SkinFirst Toothbrush | ElectraMax Speaker | UrbanStyle Wallet |
| Data-Driven | NovaTech Charger | NovaTech Charger | NovaTech Charger | NovaTech Charger |
| Impossible Task | null (correct) | Low-confidence fallback | "Unknown" (wrong) | Best-effort |

---

## Confidence Scores by Scenario

```
Quick Purchase
Single  ████████████████████████████████████████░░░░░░░░░░  80.5%
Multi   █████████████████████████████████░░░░░░░░░░░░░░░░░  67.5%
A2A     ██████████████████████████████████████░░░░░░░░░░░░  77.4%
MCP     ██████████████████████████████████████████░░░░░░░░  85.5%

Informed Decision
Single  ██████████████████████████████████████░░░░░░░░░░░░  77.9%
Multi   ████████████████████████████████████████████░░░░░░  89.7%
A2A     (Recording Error)
MCP     █████████████████████████████████████████████░░░░░  90.3%

Balanced Choice
Single  ███████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  38.3%
Multi   ████████████████████████████████░░░░░░░░░░░░░░░░░░  64.6%
A2A     ███████████████████████████████████████████████░░░  95.5%
MCP     █████████████████████████████████████████████████░  98.0%

Impossible Task
Single  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░   0% ✓
Multi   █████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  10% ✓
A2A     ███████████████████████████████████████████████░░░  95% ✗
MCP     ███████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░  47.5%
```
