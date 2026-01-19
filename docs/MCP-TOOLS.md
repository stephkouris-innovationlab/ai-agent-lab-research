# MCP Tool Simulation

This document details how we simulate MCP (Model Context Protocol) tools in this research platform.

---

## Overview

The MCP-Enhanced Agent demonstrates the **tool-use pattern** using simulated tool servers rather than real external integrations. This is an intentional pedagogical design choice that allows us to showcase how AI agents interact with external tools without requiring complex infrastructure or API keys.

In production, these simulated tools would be replaced with real MCP servers connecting to live databases, APIs, and services.

---

## Simulated Tool Servers

We implement **3 tool servers** providing **6 tools** total:

| Server | Tools | Total Latency |
|--------|-------|---------------|
| ProductDatabase | 2 | ~450ms |
| PriceTracking | 2 | ~450ms |
| ReviewAnalysis | 2 | ~650ms |

---

## 1. ProductDatabase Server

**Purpose:** Search and retrieve product information from the catalog

### Tool: `search_products`

Search the product database with advanced filtering. Returns matching products sorted by relevance and rating.

**Simulated Delay:** 300ms

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | Yes | Keywords to match in product name/category |
| `category` | string | No | Filter by category (electronics, home-kitchen, sports, books, fashion, beauty, toys, food) |
| `minPrice` | number | No | Minimum price filter |
| `maxPrice` | number | No | Maximum price filter |
| `minRating` | number | No | Minimum rating (1-5 scale) |
| `inStockOnly` | boolean | No | Only return in-stock products |
| `ecoFriendlyOnly` | boolean | No | Only return eco-friendly products |
| `limit` | number | No | Max results to return (default: 10) |

**Returns:** Array of products with id, name, brand, price, rating, availability, shipping info

---

### Tool: `get_product_details`

Get comprehensive information for a specific product by ID.

**Simulated Delay:** 150ms

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `productId` | string | Yes | The product ID to look up |

**Returns:** Full product details including specs, ratings distribution, sustainability info

---

## 2. PriceTracking Server

**Purpose:** Historical price data and deal discovery

### Tool: `get_price_history`

Get historical price data to identify trends and optimal purchase timing.

**Simulated Delay:** 250ms

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `productId` | string | Yes | The product ID to track |
| `days` | number | No | Days of history (default: 30) |

**Returns:** Price history array, statistics (lowest/highest/average), buy/wait recommendation

---

### Tool: `find_deals`

Find products with best discounts using weighted dealScore algorithm.

**Simulated Delay:** 200ms

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `category` | string | No | Filter deals by category |
| `minDiscount` | number | No | Minimum discount % (default: 10) |
| `maxPrice` | number | No | Maximum price filter |
| `limit` | number | No | Max deals to return (default: 5) |

**Returns:** Ranked deals with discount %, savings amount, and computed dealScore

**Deal Score Formula:**
```
dealScore = (discountPercent × 0.4) + (rating × 10)
```

---

## 3. ReviewAnalysis Server

**Purpose:** Sentiment analysis and product comparison

### Tool: `analyze_reviews`

Analyze customer reviews for sentiment patterns and common themes.

**Simulated Delay:** 350ms

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `productId` | string | Yes | The product ID to analyze |

**Returns:** Sentiment score/label, positive/negative themes, key insights, confidence level

**Sentiment Calculation:**
```
sentimentScore = ((positiveReviews - negativeReviews) / totalReviews + 1) × 50
```

| Score Range | Label |
|-------------|-------|
| ≥70 | Very Positive |
| ≥55 | Positive |
| ≥45 | Mixed |
| ≥30 | Negative |
| <30 | Very Negative |

---

### Tool: `compare_products`

Compare multiple products side-by-side on key metrics.

**Simulated Delay:** 300ms

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `productIds` | string | Yes | Comma-separated product IDs (max 4) |

**Returns:** Side-by-side comparison with winners for price, rating, and shipping speed

---

## Design Assumptions

### 1. Simulated Network Latency

**What:** 150-350ms artificial delays per tool call

**Why:** Real MCP servers communicate over HTTP/WebSocket with inherent network overhead. Our delays create realistic UX timing without actual network calls, making demos feel authentic.

---

### 2. Deterministic Scoring Algorithm

**What:** Weighted formula combining category, rating, price, discount, availability, eco-friendly, sentiment, and price history factors

**Why:** Ensures reproducible results for educational demos. Production systems would use ML-based ranking that adapts to user behavior and real-time signals.

**Scoring Factors:**

| Factor | Points | Condition |
|--------|--------|-----------|
| Category Match | +25 / -30 | Matches/doesn't match expected category |
| Rating | +5 to +20 | Based on star rating (3.5-4.5+) |
| Price | +10 to +15 / -10 | Within/under/over budget |
| Discount | +8 to +15 | 10%+ or 20%+ discount |
| Availability | +5 to +10 | In stock, fast shipping |
| Eco-Friendly | +5 to +20 / -10 | If required by scenario |
| Sentiment | +10 | Review sentiment ≥70 |
| Price History | +10 | At historical low |

---

### 3. 60-Product Curated Catalog

**What:** Fixed dataset vs. thousands in real e-commerce systems

**Why:** Small catalog enables full coverage of edge cases (impossible tasks, category gaps, competing priorities) while keeping demos fast and predictable.

---

### 4. 6-Tier Progressive Fallback

**What:** Search strategy that systematically relaxes constraints until results are found

**Why:** Demonstrates graceful degradation pattern that real systems need.

**Fallback Order:**
1. Strict filters (category, budget, rating, eco-friendly)
2. Relaxed eco-friendly requirement
3. Category-only filter
4. Best-rated filter (min 4.0 rating)
5. Expanded budget (×1.5)
6. Any product with min rating 3.5

---

## Simulation vs. Production

| Aspect | Our Simulation | Production MCP |
|--------|----------------|----------------|
| Data Source | 60-product mock catalog | Real-time databases (thousands of products) |
| Ranking Logic | Deterministic weighted scoring | ML-based personalized ranking |
| Network Calls | None (simulated delays) | HTTP/WebSocket to external servers |
| Latency | Fixed 150-350ms per tool | Variable based on network/load |
| Price Data | Generated historical curves | Live market prices |
| Reviews | Algorithmically generated themes | Real customer feedback with NLP |

---

## Tool Execution Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    MCP Agent Execution                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Phase 1: Product Discovery                                     │
│  └─ search_products() with progressive fallback                 │
│                                                                 │
│  Phase 2: Deal Analysis                                         │
│  └─ find_deals() to identify discounted items                   │
│                                                                 │
│  Phase 3: Detailed Analysis                                     │
│  └─ get_product_details() for top 3 candidates                  │
│                                                                 │
│  Phase 4: Review Analysis                                       │
│  └─ analyze_reviews() for top candidate                         │
│                                                                 │
│  Phase 5: Price Tracking                                        │
│  └─ get_price_history() for top candidate                       │
│                                                                 │
│  Phase 6: Comparison (if ≥2 candidates)                         │
│  └─ compare_products() for final decision                       │
│                                                                 │
│  Maximum: 8 tool calls per session                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Why Simulate?

This simulation approach provides several benefits:

1. **No API Keys Required** - Demo works without external service configuration
2. **Consistent Results** - Reproducible for educational demonstrations
3. **Fast Iteration** - No network latency during development
4. **Full Control** - Can craft specific edge cases and scenarios
5. **Cost-Free** - No external API charges during testing

The architecture and patterns demonstrated are production-ready—only the data sources need to be swapped for real MCP server integrations.

---

## See Also

- [Architecture Overview](ARCHITECTURE.md) - How MCP fits with other agent patterns
- [Methodology](METHODOLOGY.md) - How we tested the agents
- [Limitations](LIMITATIONS.md) - Research caveats including MCP simulation
