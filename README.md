# olist-sql-powerbi-analysis

# Olist E-Commerce: Delivery Performance & Customer Satisfaction

## 📌 Business Context
An e-commerce company wants to understand how delivery delays impact customer satisfaction. The goal is to prioritize logistics and operational improvements that directly affect brand reputation and customer retention.

## 🛠️ Methodology
- Extracted and prepared relational data from a relational database using **SQL (SQLite / DBeaver)**.
- Created a `JOIN` between the orders and reviews tables to calculate delivery delays (`julianday` functions).
- Grouped orders into delivery statuses (On time, 1-5 days late, 6-15 days late, 15+ days late).
- Imported the aggregated data into **Power BI** to design an interactive dashboard.

## 💡 Key Findings
- **Drastic Satisfaction Drop:** Customer satisfaction plummets when deliveries are late. Even a short delay of 1-5 days reduces the average review score from **3.77 to 2.62**.
- **Severe Reputation Risk:** Over **60,000 orders** are currently arriving late (6+ days), receiving an average rating of 1.5, which severely impacts brand reputation.

## 🚀 Recommendations
- **Proactive Communication:** Implement automated email alerts for orders at risk of delay to manage customer expectations and mitigate frustration.
- **Operational Audit:** Investigate the root causes of delays exceeding 6 days, as they result in near-perfect negative reviews.

## 🧰 Tools Used
- **SQL (SQLite / DBeaver):** Data extraction, manipulation, and `JOIN` operations.
- **Power BI:** Data modeling, DAX (Measures), and interactive dashboard design.
