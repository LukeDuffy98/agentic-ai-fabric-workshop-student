# Exercise 05 — Fabric audit and traceability (Portal-only)

## Learning objectives

- Validate that a summary table matches the raw table using read-only SQL.
- Interpret an audit table record as a “record” step in an agent workflow.
- Use the Lakehouse **SQL analytics endpoint** to verify traceability without notebook compute.

## Prereqs

- Your facilitator provides a Microsoft Fabric workspace and Lakehouse, and you have **read access**.
- The Lakehouse contains these tables: `public_holidays_raw`, `public_holidays_summary`, and `agent_run_audit`.
- You have access to a web browser.
- You do not run notebooks in this exercise.

## Estimated time

35 minutes

## Architecture sketch

- `public_holidays_raw` is the input dataset.
- `public_holidays_summary` is a facilitator-prepared aggregation output.
- `agent_run_audit` stores run metadata that makes the workflow traceable (what ran, when it ran, what input/output tables were used).

## Step-by-step

1. (Microsoft Fabric) Open https://app.fabric.microsoft.com
   - Expected result: The sign-in page appears.

2. (Microsoft Fabric) Sign in using the account provided by your facilitator.
   - Expected result: The Microsoft Fabric home page loads.

3. (Microsoft Fabric) Open the workspace provided by your facilitator.
   - Expected result: The workspace item list is visible.

4. (Microsoft Fabric) Open the Lakehouse provided by your facilitator.
   - Expected result: You can see **Tables** and **Files**.

5. (Microsoft Fabric — Lakehouse) Open the Lakehouse **SQL analytics endpoint**.
   - Expected result: The SQL analytics endpoint opens.

6. (Microsoft Fabric — Lakehouse) Select **New SQL query**.
   - Expected result: A new SQL query editor opens.

7. (Microsoft Fabric — Lakehouse) Run the SQL query below.
   - Expected result: The query returns **0 rows**.

   ```sql
   -- Validate: the stored summary matches a recomputed summary from raw data
   WITH
   computed AS (
      SELECT
         Countryorregion AS country_or_region,
         COUNT(*) AS computed_count
      FROM public_holidays_raw
      GROUP BY Countryorregion
   ),
   stored AS (
      SELECT
         country_or_region,
         [count] AS stored_count
      FROM public_holidays_summary
   )
   SELECT
      COALESCE(stored.country_or_region, computed.country_or_region) AS country_or_region,
      stored.stored_count,
      computed.computed_count
   FROM stored
   FULL OUTER JOIN computed
      ON stored.country_or_region = computed.country_or_region
   WHERE ISNULL(stored.stored_count, -1) <> ISNULL(computed.computed_count, -1);
   ```

8. (Microsoft Fabric — Lakehouse) Run the SQL query below.
   - Expected result: You can see the most recent audit record, including `input_table` and `output_table`.

   ```sql
   SELECT TOP 1
     run_timestamp_utc,
     run_id,
     input_table,
     output_table,
     input_row_count,
     output_row_count,
     workflow_version
   FROM agent_run_audit
   ORDER BY run_timestamp_utc DESC;
   ```

9. (Microsoft Fabric — Lakehouse) Run the SQL query below.
   - Expected result: `raw_row_count` and `summary_row_count` are both greater than 0.

   ```sql
   SELECT
     (SELECT COUNT(*) FROM public_holidays_raw) AS raw_row_count,
     (SELECT COUNT(*) FROM public_holidays_summary) AS summary_row_count;
   ```

## Validation

- The “stored vs computed” comparison query returns 0 rows.
- `agent_run_audit` returns at least one record.
- Row counts for `public_holidays_raw` and `public_holidays_summary` are greater than 0.

## Cleanup

1. (Microsoft Fabric) Close the browser tab.
   - Expected result: No shared lab resources are changed.

2. (Microsoft Fabric) Do not delete any pipelines, notebooks, or Lakehouse tables in the shared lab workspace.
   - Expected result: The environment remains available for other students.

## Compliance / safety notes

- Use only synthetic or sample data in this exercise.
- Do not paste secrets, tokens, or personal data into any query.

## References

- https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-sql-analytics-endpoint
- https://learn.microsoft.com/en-us/fabric/data-warehouse/sql-query-editor
