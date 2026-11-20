Data Analytics Projects

Two end-to-end data analytics projects, submitted together for the Data Analytics track.

Author: Mandira Biswas | Submission date: 20 Nov 2026

#	Project	What it does	Stack	Folder
1	Retail & Supply Chain Control Tower (DA-03)	Near real-time monitoring of stock, suppliers and demand for a 10-store retail chain: hourly pipeline, KPIs, anomaly detection, demand forecast, what-if dashboard and alerts	PostgreSQL, Airflow, Python (pandas, scikit-learn, Prophet), Superset, Docker	project-1-supply-chain-control-tower/
2	Enterprise Business Intelligence Suite (DA-01)	One Power BI report combining sales, finance, HR and supply chain, with regional row-level security and automated daily refresh	SQL Server, SQL Server Agent, Power BI (Power Query, DAX, RLS), Power BI Service + gateway	project-2-enterprise-bi-powerbi/

Each folder has its own README with architecture, setup steps, test results and limitations, plus an individual REFLECTION.md.

Project 1: Retail & Supply Chain Control Tower

Architecture

Pipeline: Airflow DAG runs hourly, ingests the next hour of data into PostgreSQL, then checks alert rules (13 runs, 0 failures).
KPIs (SQL views): stock-out rate 11.07%, on-time delivery 57.66%, supplier lead time 41.3 h, days of inventory 3.0.
Anomaly detection: seasonal z-score (318 demand spikes) and Isolation Forest.
Forecast: Prophet, 7 days hourly per product; backtest WAPE 21.9%.
What-if panel: at-risk store-products rise from 54 to 102 as demand and supplier delays increase.
Alerts: 5 threshold rules, logged every hour; optional Slack/email. 10 unit tests, all passing.

Details: Project 1 README · Test results · Reflection

Project 2: Enterprise Business Intelligence Suite

Architecture

Star schema: 5 fact tables (sales, SQL summary, finance, HR, inventory) around shared dimensions; snowflake tables merged in Power Query.
6 report pages: Executive Summary, Product Details (drill-through), Sales Trends (YTD, YoY, rolling 3-month average), Finance (actual vs budget), HR, Supply Chain.
Row-level security: Europe / North America / Pacific roles across sales, finance and HR; the three regions add up exactly to the unfiltered total.
Automation: SQL Server Agent job rebuilds a summary table daily ("invoked by Schedule"); Power BI Service refreshes daily through an on-premises gateway.
13 tests passed (totals, time intelligence, drill-through, RLS, job and refresh).

Details: Project 2 README · User guide · Data dictionary · Reflection

Running the projects
Project 1: needs Docker Desktop and Python 3.10+. Copy .env.example to .env, run docker compose up -d --build, then follow the setup section of its README.
Project 2: needs SQL Server with AdventureWorksDW2022 and Power BI Desktop. Open Supply_Chain_Sales_Dashboard.pbix and follow the setup section of its README.
Repository layout
README.md                                  this overview
project-1-supply-chain-control-tower/      Docker stack, Airflow DAGs, Python models, SQL views, tests, docs
project-2-enterprise-bi-powerbi/           Power BI report, SQL script, user guide, docs and screenshots