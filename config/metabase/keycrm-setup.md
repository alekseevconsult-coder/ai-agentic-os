# Metabase KeyCRM Dashboard Setup

## Connect to Analytics Database

1. Admin → Databases → Add database
2. Fill in:
   - Type: PostgreSQL
   - Name: KeyCRM Analytics
   - Host: postgres
   - Port: 5432
   - Database: analytics
   - Username: [your POSTGRES_USER from .env]
   - Password: [your POSTGRES_PASSWORD from .env]

## Recommended Dashboards to Build

### Dashboard 1: Sales Overview
- Revenue by month (line chart) — keycrm_payments
- Orders count by month (bar chart) — keycrm_orders
- Average order value (KPI number)
- Total revenue MTD (KPI number)

### Dashboard 2: Product Performance
- Top 20 SKUs by revenue (bar chart) — keycrm_order_products
- Top 20 SKUs by quantity sold
- Revenue by product category

### Dashboard 3: Customer Analytics
- New customers per month
- Top 20 customers by spend — keycrm_buyers
- Customer retention (repeat order rate)

### Dashboard 4: Operations
- Orders by status (pie chart) — keycrm_orders
- Orders by source/channel
- Pending orders list (table with filters)
- Sync health — keycrm_sync_log

## Enable Scheduled Email Reports

Metabase → Settings → Email → Configure SMTP, then:
Dashboard → top right → Subscriptions → Add email subscription
Set frequency: daily at 8am

This gives you an automated morning sales briefing by email.
