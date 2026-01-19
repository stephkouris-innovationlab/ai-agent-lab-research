# Product Catalog

The benchmark testing uses a curated catalog of 60 synthetic products designed to enable meaningful agent comparisons.

---

## Overview

| Metric | Value |
|--------|-------|
| Total Products | 60 |
| Categories | 8 |
| Price Range | $15 - $350 |
| Average Rating | 4.2 stars |

---

## Categories

| Category | Count | Price Range | Examples |
|----------|-------|-------------|----------|
| Electronics | 9 | $15 - $399 | Chargers, headphones, speakers, webcams |
| Home Appliances | 7 | $49 - $699 | Coffee makers, vacuums, air purifiers |
| Fashion | 7 | $15 - $399 | T-shirts, jeans, wallets, boots |
| Books & Media | 6 | $12 - $149 | Business books, tech guides, courses |
| Groceries | 7 | $12 - $59 | Coffee, snacks, supplements |
| Furniture | 7 | $79 - $1,999 | Office chairs, desks, sofas |
| Sports & Outdoors | 7 | $19 - $499 | Yoga mats, dumbbells, camping gear |
| Beauty | 7 | $14 - $199 | Skincare, haircare, oral care |

---

## Product Structure

Each product contains:

```json
{
  "id": "electronics-3",
  "name": "NovaTech USB-C Fast Charger",
  "description": "High-quality USB-C fast charger...",
  "category": "electronics",
  "subcategory": "Accessories",
  "brand": "NovaTech",
  "model": "NO-4521",
  "price": {
    "current": 21.94,
    "original": 27.43,
    "discountPercent": 20
  },
  "ratings": {
    "average": 4.6,
    "count": 287,
    "distribution": { "1": 5, "2": 12, "3": 28, "4": 72, "5": 170 }
  },
  "reviews": [...],
  "specifications": {
    "Power": "65W",
    "Ports": "2",
    "Protocol": "PD 3.0"
  },
  "availability": {
    "inStock": true,
    "quantity": 47,
    "shippingDays": 3,
    "freeShipping": true
  },
  "seller": {
    "name": "NovaTech Official Store",
    "rating": 4.7,
    "returnPolicy": "30-day returns",
    "shippingTime": "2-3 days"
  },
  "sustainability": {
    "carbonFootprint": "low",
    "recyclability": "partial",
    "ecoFriendly": true,
    "certifications": ["Energy Star"]
  },
  "tags": ["electronics", "accessories", "novatech", "eco-friendly"]
}
```

---

## Electronics (9 products)

| Product | Brand | Price | Rating | Key Specs |
|---------|-------|-------|--------|-----------|
| Wireless Noise-Canceling Headphones | Various | $79-349 | 4.0-4.8 | 30hr battery, 40mm drivers, ANC |
| Work-From-Home Headset Pro | DigitalEdge | $129-249 | 4.2-4.7 | Boom mic, 24hr battery, multipoint |
| Portable Bluetooth Speaker | ElectraMax | $29-199 | 3.5-4.6 | IPX7 waterproof, 12hr battery |
| USB-C Fast Charger | NovaTech | $15-28 | 4.0-4.8 | 65W, PD 3.0, dual port |
| 4K Webcam with Microphone | Various | $49-179 | 4.0-4.5 | 4K/60fps, 90° FOV |
| Mechanical Gaming Keyboard | Various | $59-199 | 4.2-4.7 | Cherry MX, RGB backlight |
| Wireless Gaming Mouse | Various | $39-149 | 4.0-4.6 | 25600 DPI, 70hr battery |
| Portable SSD 1TB | Various | $79-159 | 4.3-4.8 | 1050 MB/s, USB 3.2 |
| Smart Watch Fitness Tracker | Various | $99-399 | 4.0-4.5 | 7-day battery, GPS |

---

## Home Appliances (7 products)

| Product | Brand | Price | Rating | Key Specs |
|---------|-------|-------|--------|-----------|
| Programmable Coffee Maker | HomeElite | $49-199 | 4.0-4.6 | 12 cups, timer |
| Robot Vacuum Cleaner | CleanMaster | $199-699 | 4.2-4.7 | LiDAR mapping, 150min |
| Air Purifier HEPA Filter | AirFlow | $99-399 | 4.1-4.6 | 500 sq ft, 25dB |
| Instant Pot Multi-Cooker | KitchenPro | $79-149 | 4.4-4.8 | 6qt, 7-in-1 |
| Stand Mixer Professional | KitchenPro | $199-499 | 4.3-4.7 | 500W, 5qt bowl |
| Cordless Vacuum Stick | CleanMaster | $149-449 | 4.0-4.5 | 60min runtime |
| Smart Thermostat | Various | $99-249 | 4.2-4.6 | Alexa/Google, 23% savings |

---

## Fashion (7 products)

| Product | Brand | Price | Rating | Key Specs |
|---------|-------|-------|--------|-----------|
| Classic Cotton T-Shirt | UrbanStyle | $15-45 | 4.0-4.5 | 100% cotton |
| Sustainable Denim Jeans | NatureWear | $59-149 | 4.1-4.6 | Recycled denim |
| Waterproof Hiking Boots | TrailBlazer | $89-229 | 4.2-4.7 | Gore-Tex, Vibram sole |
| Leather Minimalist Wallet | UrbanStyle | $29-99 | 4.3-4.8 | RFID blocking |
| Wool Blend Winter Coat | ClassicFit | $149-399 | 4.0-4.5 | 70% wool |
| Athletic Running Shoes | ActiveMove | $79-179 | 4.1-4.6 | 280g, 8mm drop |
| Bamboo Fiber Socks Pack | NatureWear | $19-39 | 4.2-4.7 | 6 pairs, antibacterial |

---

## Books & Media (6 products)

| Product | Brand | Price | Rating | Key Specs |
|---------|-------|-------|--------|-----------|
| Leadership & Management Guide | PageTurner | $14-29 | 4.3-4.7 | 320 pages, hardcover |
| Learn Python Programming | LearnFast | $29-49 | 4.4-4.8 | 450 pages, 200+ exercises |
| Meditation & Mindfulness | MindGrow | $12-24 | 4.2-4.6 | Audio included |
| Sci-Fi Novel Collection | StoryVerse | $24-44 | 4.5-4.9 | 3 books, Hugo winner |
| Documentary Film Box Set | Various | $29-59 | 4.1-4.5 | 4 discs, 4K UHD |
| Language Learning Course | LearnFast | $49-149 | 4.0-4.4 | 500+ lessons, 1yr access |

---

## Groceries (7 products)

| Product | Brand | Price | Rating | Key Specs |
|---------|-------|-------|--------|-----------|
| Organic Coffee Beans 1lb | FreshFarm | $12-24 | 4.3-4.7 | USDA certified, Colombia |
| Mixed Nuts & Trail Mix | NutriBest | $14-28 | 4.2-4.6 | 2lb, 6 varieties |
| Protein Powder Vanilla | NutriBest | $29-59 | 4.1-4.5 | 30 servings, 25g protein |
| Extra Virgin Olive Oil | GreenChoice | $15-35 | 4.4-4.8 | 750ml, cold pressed |
| Green Tea Variety Pack | OrganicLife | $12-24 | 4.2-4.6 | 100 bags, 5 varieties |
| Dark Chocolate Assortment | FreshFarm | $18-36 | 4.5-4.9 | 24 pieces, 70% cocoa |
| Vitamin D3 Supplements | NutriBest | $12-24 | 4.0-4.4 | 180 softgels |

---

## Furniture (7 products)

| Product | Brand | Price | Rating | Key Specs |
|---------|-------|-------|--------|-----------|
| Ergonomic Office Chair | ComfortZone | $199-599 | 4.2-4.7 | Mesh, adjustable lumbar |
| Standing Desk Electric | ModernSpace | $299-699 | 4.3-4.8 | 60x30", dual motor |
| Bookshelf 5-Tier Wood | WoodCraft | $79-199 | 4.1-4.5 | Solid wood |
| Memory Foam Mattress Queen | ComfortZone | $399-999 | 4.4-4.8 | 12", gel-infused |
| Sectional Sofa L-Shaped | StyleHome | $799-1,999 | 4.0-4.5 | 5 seats, reversible |
| Dining Table Set 6-Person | WoodCraft | $499-1,299 | 4.2-4.6 | Wood, extendable |
| Nightstand with Drawers | EcoFurnish | $79-199 | 4.1-4.5 | USB port built-in |

---

## Sports & Outdoors (7 products)

| Product | Brand | Price | Rating | Key Specs |
|---------|-------|-------|--------|-----------|
| Yoga Mat Premium | FitGear | $29-89 | 4.3-4.7 | 6mm, TPE, non-slip |
| Adjustable Dumbbell Set | SportMax | $199-499 | 4.4-4.8 | 5-52.5lb, pair |
| Camping Tent 4-Person | TrailBlazer | $99-299 | 4.1-4.5 | 3-season, 5min setup |
| Hiking Backpack 50L | OutdoorPro | $79-199 | 4.2-4.6 | Rain cover included |
| Resistance Bands Set | FitGear | $19-49 | 4.0-4.4 | 5 bands, 10-50lb |
| Insulated Water Bottle 32oz | ActiveLife | $24-44 | 4.3-4.7 | 24hr cold, stainless |
| Running GPS Watch | SportMax | $149-399 | 4.2-4.6 | Multi-band GPS, 14-day |

---

## Beauty (7 products)

| Product | Brand | Price | Rating | Key Specs |
|---------|-------|-------|--------|-----------|
| Vitamin C Serum | GlowUp | $19-49 | 4.2-4.6 | 20%, 30ml |
| Retinol Night Cream | SkinFirst | $29-69 | 4.1-4.5 | 0.5%, fragrance-free |
| Hair Dryer Ionic | BeautyLab | $39-149 | 4.0-4.4 | 1875W, ionic |
| Electric Toothbrush | SkinFirst | $49-199 | 4.3-4.7 | 5 modes, smart timer |
| Sunscreen SPF 50 | NaturalBeauty | $14-34 | 4.2-4.6 | Mineral, 100ml |
| Essential Oil Diffuser | PureEssence | $24-59 | 4.1-4.5 | 300ml, 10hr |
| Makeup Brush Set | BeautyLab | $19-59 | 4.0-4.4 | 12 brushes, case |

---

## Catalog Design Principles

### 1. Scenario Coverage

Products were selected to enable testing of all five scenarios:
- **Quick Purchase:** USB-C chargers with clear specs
- **Informed Decision:** Work headphones with multiple factors
- **Balanced Choice:** Gifts spanning eco-friendly, quality, and price
- **Data-Driven:** Electronics with price history potential
- **Impossible Task:** No products meet contradictory requirements

### 2. Trade-off Enablement

Multiple products exist in similar categories with different trade-offs:
- High quality / High price
- Budget-friendly / Fewer features
- Fast shipping / Higher cost
- Eco-friendly / Premium price

### 3. Realistic E-commerce

Products mirror real online shopping:
- Discounts and original prices
- Review distributions (not all 5-star)
- Shipping times vary
- Seller ratings and return policies

### 4. Deterministic Generation

Products are generated with a seeded random number generator for reproducibility. Running the generation code always produces the same 60 products.

---

## Data Generation

Products are generated programmatically with:

```typescript
// Seeded for determinism
const seededRandom = createSeededRandom(42);

// Template-based generation
const productTemplates = {
  electronics: [
    { name: 'Wireless Headphones', priceRange: [79, 349], ... },
    ...
  ],
  ...
};
```

This ensures:
- Consistent product IDs across runs
- Reproducible benchmark comparisons
- No external data dependencies
