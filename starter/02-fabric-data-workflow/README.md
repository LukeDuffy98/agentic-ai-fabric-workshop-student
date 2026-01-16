# Exercise 02 — Fabric data workflow (Portal-only)

## Learning objectives

- Review a successful Data Factory pipeline run in Microsoft Fabric.
- Preview Lakehouse tables produced by a facilitator-prepared workflow.
- Validate row counts using the Lakehouse **SQL analytics endpoint**.

## Prereqs

- Your facilitator provides a Microsoft Fabric workspace and Lakehouse, and you have **read access**.
- The workspace contains a pipeline named `01_Load_PublicHolidays` with a recent **Succeeded** run.
- The Lakehouse contains these tables: `public_holidays_raw`, `public_holidays_summary`, and `agent_run_audit`.
- You have access to a web browser.
- You do not run notebooks in this exercise.

## Estimated time

35 minutes

## Architecture sketch

- A Data Factory pipeline loads sample data into `public_holidays_raw`.
- A facilitator-run process produces `public_holidays_summary` and appends a row to `agent_run_audit`.
- You validate outputs using table preview and SQL (read-only).

## Step-by-step

1. (Microsoft Fabric) Open https://app.fabric.microsoft.com
   - Expected result: The sign-in page appears.

2. (Microsoft Fabric) Sign in using the account provided by your facilitator.
   - Expected result: The Microsoft Fabric home page loads.

3. (Microsoft Fabric) Open the workspace provided by your facilitator.
   - Expected result: The workspace item list is visible.

4. (Microsoft Fabric) Switch to the **Data Factory** experience.
   - Expected result: The Data Factory experience loads.

5. (Microsoft Fabric — Data Factory) In the workspace item list, select the pipeline `01_Load_PublicHolidays`.
   - Expected result: The pipeline details open.

6. (Microsoft Fabric — Data Factory) Open the pipeline **Output** tab.
   - Expected result: You can see one or more pipeline activity output rows.

7. (Microsoft Fabric — Data Factory) Confirm the most recent run shows **Succeeded**.
   - Expected result: The status is **Succeeded**.

8. (Microsoft Fabric — Data Factory) In the most recent output row, select **Run details**.
   - Expected result: The run details page opens and shows row counts and duration.

9. (Microsoft Fabric) Return to the workspace item list.
   - Expected result: The workspace item list is visible.

10. (Microsoft Fabric) Open the Lakehouse provided by your facilitator.
   - Expected result: The Lakehouse opens.

11. (Microsoft Fabric — Lakehouse) Under **Tables**, select `public_holidays_raw`.
   - Expected result: The table page opens.

12. (Microsoft Fabric — Lakehouse) Select **Preview**.
   - Expected result: You can see rows and a column named `Countryorregion`.

13. (Microsoft Fabric — Lakehouse) Under **Tables**, select `public_holidays_summary`.
   - Expected result: The table page opens.

14. (Microsoft Fabric — Lakehouse) Select **Preview**.
   - Expected result: You can see a summary of countries/regions and counts.

15. (Microsoft Fabric — Lakehouse) Under **Tables**, select `agent_run_audit`.
   - Expected result: The table page opens.

16. (Microsoft Fabric — Lakehouse) Select **Preview**.
   - Expected result: You can see audit rows including `run_timestamp_utc`, `input_table`, and `output_table`.

17. (Microsoft Fabric — Lakehouse) Open the Lakehouse **SQL analytics endpoint**.
   - Expected result: The SQL analytics endpoint opens.

18. (Microsoft Fabric — Lakehouse) Select **New SQL query**.
   - Expected result: A new SQL query editor opens.

19. (Microsoft Fabric — Lakehouse) Run the SQL query below.
   - Expected result: Each table returns a row count greater than 0.

   ```sql
   SELECT 'public_holidays_raw' AS table_name, COUNT(*) AS row_count FROM public_holidays_raw
   UNION ALL
   SELECT 'public_holidays_summary' AS table_name, COUNT(*) AS row_count FROM public_holidays_summary
   UNION ALL
   SELECT 'agent_run_audit' AS table_name, COUNT(*) AS row_count FROM agent_run_audit;
   ```

20. (Microsoft Fabric — Lakehouse) Run the SQL query below.
   - Expected result: You can see the most recent audit record (ordered by `run_timestamp_utc`).

   ```sql
   SELECT TOP 5
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

## Validation

- `public_holidays_raw` exists and previews successfully.
- `public_holidays_summary` exists and previews successfully.
- `agent_run_audit` exists and contains at least one row.

## Cleanup

1. (Microsoft Fabric) Close the browser tab.
   - Expected result: No shared lab resources are changed.

2. (Microsoft Fabric) Do not delete any pipelines, notebooks, or Lakehouse tables in the shared lab workspace.
   - Expected result: The environment remains available for other students.

## Compliance / safety notes

- Use only synthetic or sample data in this exercise.
- Do not paste secrets, tokens, or personal data into any query.
- Do not run notebooks in a shared classroom capacity unless your facilitator explicitly instructs you to.

## References

- https://learn.microsoft.com/en-us/fabric/data-factory/tutorial-load-data-lakehouse-transform
- https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-sql-analytics-endpoint
- https://learn.microsoft.com/en-us/fabric/data-warehouse/sql-query-editor
