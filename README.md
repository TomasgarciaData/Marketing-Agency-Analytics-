# Marketing-Agency-Analytics-
End-to-end data pipeline built on Microsoft Fabric for multi-client marketing analytics. Ingests data from paid ads, organic social and e-commerce platforms, processes it through a medallion architecture, and serves clean gold tables ready for reporting in Power BI.

---

## Workspace Structure

| Item | Type | Description |
|---|---|---|
| `Ingest Data` | Notebook | Fetches raw data from each platform API and saves to bronze |
| `Not Implemented` | Notebook | Bronze → Silver transformation and upsert |
| `Data parse marketing` | Notebook | Silver → Gold transformation and upsert |
| `Pipeline_Data` | Pipeline | Orchestrates the full run in sequence |
| `marketing_clients` | Lakehouse | Stores all bronze files and Delta tables |

---

## Data Sources

| Platform | Type | Status |
|---|---|---|
| Meta Ads | Paid Ads | ✅ Active |
| Meta Organic | Organic Social | ✅ Active |
| Shopify | E-commerce | ✅ Active |
| TikTok Ads | Paid Ads | ⬜ Implemented, inactive |
| TikTok Organic | Organic Social | ⬜ Implemented, inactive |
| Google Ads | Paid Ads | 🔲 Pending |
| Tiendanube | E-commerce | ⬜ Implemented, inactive |

---

## Gold Tables

| Table | Description |
|---|---|
| `gold_ads` | Unified paid ad insights across all platforms |
| `gold_organic` | Organic post performance across all platforms |
| `gold_ecommerce_orders` | Order line items with date/time split |
| `gold_ecommerce_products` | Product catalog with pricing and inventory |
| `gold_ecommerce_customers` | Customer profiles with purchase summary |
| `calendario` | Date dimension (2025–2026) |

---

## Multi-Client Design

Each client gets its own lakehouse schema. Adding a new client requires a single
entry in the `CLIENTES` registry — no pipeline changes needed.

```python
CLIENTES = {
    "<client_slug>": {
        "nombre": "<Client Display Name>",
        "schema": "<lakehouse_schema>",
        "industria": "<industry>",
        "fuentes": {
            "meta": {"activo": True},
            "shopify": {"activo": True},
            "google_ads": {"activo": False},
            "tiktok": {"activo": False}
        }
    }
}
```

---

## Setup

1. Clone this repository into your Microsoft Fabric workspace.
2. Copy `config.example.py` to `config.py` and fill in your API credentials.
3. Attach the `marketing_clients` lakehouse to each notebook.
4. Run `Pipeline_Data` to execute the full ingestion and transformation flow.

> **Note:** `config.py` is listed in `.gitignore`. Never commit real credentials.

---

## Tech Stack

- **Microsoft Fabric** — Lakehouse, Notebooks, Pipelines, SQL Analytics Endpoint
- **PySpark** — distributed data processing
- **Delta Lake** — ACID transactions, upserts via merge
- **Python** — API ingestion (Meta Graph API, Shopify Admin API)
- **Power BI** — reporting layer connected to gold tables
