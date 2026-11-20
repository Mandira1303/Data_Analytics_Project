Individual Reflection: Enterprise Business Intelligence Suite (Power BI)
What I built

I built a single Power BI report on the AdventureWorksDW2022 warehouse that brings together sales, finance, HR and supply chain data. I modelled it as a star schema, wrote the DAX measures, set up row-level security for three regions, created a SQL Server Agent job that rebuilds a sales summary table every day, and published the report to Power BI Service with a daily scheduled refresh through an on-premises data gateway.

What went well
The data model. The original product and customer tables were snowflaked. Merging them in Power Query into a clean star schema made the relationships simpler and every measure easier to reason about. I checked that totals did not change after the merge (Total Sales stayed at $29,358,677 and margin at 41.15%).
Testing the security properly. Instead of only looking at one role, I compared each role's total. Europe, North America and Pacific added up exactly to the unfiltered total, which showed that no rows were lost or double-counted.
Proving the automation works. The SQL job history shows runs "invoked by Schedule", and the Service shows successful scheduled refreshes, so the report updates without manual steps.
Challenges and how I solved them
A misleading finance number. My first revenue variance showed actuals 329% above budget. Looking at the chart by year, I found budgets only exist for 2010–2011 while actuals run to 2013. I rewrote the measure so it compares actuals only for periods and divisions that have a budget. The real figure was about 9.6% below budget. This taught me to sanity-check a KPI against the underlying data before trusting it.
Stock is a snapshot, not a flow. Adding up daily stock balances gave meaningless numbers. I wrote the stock measures to take the balance on the latest day in the current filter instead.
Security for finance data. Finance organizations had no region, so the existing Territory roles did not filter them. I added a calculated Region column to the Organization table and added a second rule to each role.
The scheduled job seemed to stop. For a week the job history showed no runs. The Agent service and schedule were fine; the laptop had simply been off at the scheduled times. I set the Agent to start automatically and documented this as a limitation.
Drill-through and measure mistakes in Power BI Desktop. Drill-through stopped working because the target page used a field no visual on the summary page had, and I overwrote measures by pasting into the formula bar instead of creating a new measure. Both were small, but they taught me to check each change before moving on.
What I learned
How to design a star schema and why it matters for DAX.
DAX filter context and time intelligence (YTD, prior year, rolling averages), and when a measure needs to work on a snapshot.
Static row-level security across several fact tables, and how to test it.
How a personal gateway connects Power BI Service to a local SQL Server, and how scheduled refresh depends on it.
What I would do differently
Plan the full model (all four business areas) before building report pages, instead of adding finance, HR and inventory later.
Use Azure Data Factory and Azure SQL so refreshes do not depend on my laptop being switched on.
Commit to GitHub after every working step, with clear messages, instead of in large batches.
Next steps

Dynamic row-level security based on the user's email, incremental refresh for the large fact tables, and a customer drill-through page.