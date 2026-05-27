# 🛒 Synthetic E-Commerce Database — SQLite & Python

A relational SQLite database simulating a realistic e-commerce platform, built entirely from scratch using Python. Covers database design, synthetic data generation, schema constraints, and SQL analytics.

---

## 📁 Repository Structure

```
├── Ecommerce_Project.ipynb        # Notebook: data generation + SQL queries
├── ecommerce_project.db           # Generated SQLite database
├── .gitignore
└── README.md
```

---

## 📌 Project Overview

This project designs and populates a five-table relational SQLite database for an online retail store. All data is **100% synthetically generated** using Python — no real customer data was used at any point.

The database simulates real-world e-commerce scenarios including:
- Customer browsing and purchasing
- Order tracking with peak/off-peak shipping
- Product catalogue management
- Post-purchase product reviews

---

## 🗄️ Database Schema

Five relational tables connected via foreign keys:

| Table | Description | Rows |
|---|---|---|
| `customers` | Customer demographics and loyalty info | 1,200 |
| `products` | Product catalogue with pricing | 250 |
| `orders` | Customer orders with status and shipping | 3,200 |
| `order_items` | Bridge table (orders ↔ products) — composite PK | ~6,000 |
| `reviews` | Star ratings and optional review text | 1,800 |

### Key Schema Features
- **Primary keys** on all tables
- **Composite primary key** on `order_items` (`order_id`, `product_id`)
- **Foreign keys** with `ON DELETE CASCADE` (GDPR right to erasure)
- **CHECK constraints** — age (16–90), review score (1–5), non-negative prices
- **UNIQUE constraint** on customer email (nullable)
- **NOT NULL constraints** on all essential fields

### Data Types Covered
| Scale | Example Columns |
|---|---|
| Nominal | `gender`, `country`, `category`, `shipping_method` |
| Ordinal | `loyalty_tier` (Bronze→Platinum), `review_score` (1–5) |
| Interval | `signup_date`, `order_date`, `delivery_temp_c` |
| Ratio | `age`, `unit_price`, `quantity`, `line_total`, `shipping_fee` |

---

## 🐍 Data Generation Approach

All data was generated programmatically using Python — no external datasets were downloaded.

**Libraries used:**
- `Faker (en_GB)` — realistic UK customer names and email addresses
- `random` — weighted probability distributions
- `sqlite3` — database creation and insertion
- `pandas` — validation queries and display

**Realism features deliberately included:**
- ~5% of customer emails set to `NULL` (incomplete records)
- ~20% of orders have `NULL` discount (no promotion applied)
- ~40% of reviews have no review text (rating only)
- ~3% of customer names are intentional duplicates (common names)
- Customer ages follow a **normal distribution** (mean ≈ 31, clamped 16–90)
- Product prices follow a **skewed distribution** (Electronics category inflated)
- Order statuses are **weighted** (65% Delivered, reflecting real patterns)

---

## 🔍 Example SQL Queries

### Top customers by order count
```sql
SELECT c.customer_id, c.full_name, c.loyalty_tier, COUNT(o.order_id) AS n_orders
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.customer_id
ORDER BY n_orders DESC
LIMIT 10;
```

### Revenue by product category
```sql
SELECT p.category, ROUND(SUM(oi.line_total), 2) AS revenue
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
GROUP BY p.category
ORDER BY revenue DESC;
```

### Average review score by category
```sql
SELECT p.category,
       ROUND(AVG(r.review_score), 2) AS avg_score,
       COUNT(r.review_id) AS total_reviews
FROM reviews r
JOIN products p ON r.product_id = p.product_id
GROUP BY p.category
ORDER BY avg_score DESC;
```

---

## 🔒 Ethics & Data Privacy

All data in this database is entirely synthetic. No real personal information was collected or stored at any point.

- **GDPR compliant** — all names, emails, and locations are AI-generated via `Faker`
- **Right to erasure** — implemented via `ON DELETE CASCADE` on `order_items`; deleting a customer removes all their associated orders and items
- **Data minimisation** — no full card numbers, real dates of birth, or unnecessary personal fields are stored
- **No privacy risk** — synthetic data allows realistic analysis without legal or ethical exposure

---

## ▶️ How to Run

1. **Clone the repository**
   ```bash
   git clone https://github.com/AjayiIsrael/Ecommerce-SQLite-Database.git
   cd Ecommerce-SQLite-Database
   ```

2. **Install dependencies**
   ```bash
   pip install faker pandas
   ```

3. **Open the notebook in Google Colab or Jupyter**
   ```bash
   jupyter notebook Ecommerce_Project.ipynb
   ```

4. **Run all cells** — the database file `ecommerce_project.db` will be generated automatically.

> The pre-generated `ecommerce_project.db` is also included in the repo and can be opened directly with any SQLite viewer (e.g. [DB Browser for SQLite](https://sqlitebrowser.org/)).

---

## 👤 Author

**Israel Ajayi**  

