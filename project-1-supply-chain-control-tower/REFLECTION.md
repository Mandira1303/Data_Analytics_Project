# Individual Reflection: Retail & Supply Chain Control Tower

## What I built

I built a near real-time control tower for a simulated retail chain of 10 stores, 20 products and 5 suppliers. A Python generator creates realistic hourly data in PostgreSQL, an Airflow DAG adds a new hour of data every hour and checks alert rules, SQL views calculate the supply chain KPIs, Python scripts detect anomalies and forecast demand with Prophet, and a Superset dashboard shows everything with a what-if scenario filter. The whole stack runs in Docker.

## What went well

- **Realistic, reproducible data.** With a fixed random seed, the generator produces daily and weekly demand patterns, store size effects and supplier delays. I deliberately injected demand spikes so I had something real for anomaly detection to find.
- **One-command setup.** `docker compose up` starts PostgreSQL, Airflow and Superset together, which made the system easy to restart and demonstrate.
- **A reliable pipeline.** The hourly DAG ran 13 times with no failures, and a failed Slack or email notification is caught so it can never break the pipeline.
- **Honest forecast evaluation.** Instead of only producing a forecast, I held back the last 7 days, predicted them and measured the error (WAPE about 21.9%).

## Challenges and how I solved them

- **Choosing anomaly methods.** A plain z-score flagged too many quiet hours. Comparing each hour with the normal level for the same store, product, hour and weekday, plus a minimum sales level, made the results much more meaningful. I also learned that the Isolation Forest count is set by its contamination parameter, so it should not be presented as a finding on its own.
- **Ports and connections between containers and my laptop.** PostgreSQL inside Docker uses port 5432, but from my laptop it is 5433. Mixing these up caused connection errors until I documented it clearly.
- **Alerts not reaching Slack or email.** The alert code supported Slack and email, but the settings were never passed into the Airflow container, so notifications could not be sent. I added them to `docker-compose.yml` and an `.env.example` template. Alerts are always saved to the `alerts` table.
- **Protecting secrets.** A typo in `.gitignore` meant `.env` (with my database password) was not ignored. I caught it before it was committed, fixed the file and added `.env.example` instead.
- **Interpreting KPIs.** On-time delivery came out around 58%, lower than the supplier reliability settings. Reading my own simulator, I found that even "on-time" shipments can arrive slightly late, so I documented the difference rather than presenting it as a real supplier problem.

## What I learned

- Orchestrating a pipeline with Airflow and designing tasks so one failure does not break everything.
- Writing KPI logic as SQL views so dashboards stay current without extra code.
- Two different approaches to anomaly detection and their trade-offs.
- Time series forecasting with Prophet and how to evaluate it with a backtest.
- Writing unit tests for business rules (10 tests for the alert rules, all passing).

## What I would do differently

- Connect Slack alerts early and test them end to end, instead of leaving notification for the end.
- Run anomaly detection and forecasting inside the Airflow DAG, not as separate scripts.
- Compare Prophet with a simple seasonal baseline to show whether it really adds value.
- Commit more often with clearer messages.

## Next steps

Use real or public retail data, move Airflow to PostgreSQL metadata and LocalExecutor, add a supplier-delay anomaly rule, and send the daily summary to Slack.
