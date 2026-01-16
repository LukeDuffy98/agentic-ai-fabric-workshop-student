# Agentic AI & Fabric Workshop (Portal-only)

## Learning objectives

- Describe an “agentic AI” workflow as a repeatable loop (plan → act → record).
- Use Microsoft Fabric (Data Factory + Lakehouse) to load data and validate results in the portal.
- Review a worked example of deterministic outputs and an audit trail stored in the Lakehouse.

## Prereqs

- A facilitator-provided Microsoft Fabric environment on Fabric capacity.
- A facilitator-provided student account that can view items in the lab workspace.
- The facilitator provides these exact values:
	- **Workspace name:** `<<LAB_WORKSPACE_NAME>>`
	- **Lakehouse name:** `<<LAB_LAKEHOUSE_NAME>>`

- The facilitator has already prepared the demo environment (students will not run compute):
	- A pipeline named `01_Load_PublicHolidays` exists and has a recent **Succeeded** run.
	- The Lakehouse contains `public_holidays_raw`, `public_holidays_summary`, and `agent_run_audit`.

- You must have **read access** to the demo workspace and Lakehouse.
- You have access to a web browser.

## Estimated time

- 75–90 minutes

## Architecture sketch

- A Data Factory pipeline loads sample data into a Lakehouse table.
- A facilitator-run notebook demo reads that input table, produces a summary table, and appends an audit record.
- Students review the demo output and audit table to understand traceability: what ran, when it ran, what input was used, and what output was produced.

## Step-by-step

1. **Where:** Browser
	- Open `https://app.fabric.microsoft.com`.
	- **Expected result:** The sign-in page appears.

2. **Where:** Browser
	- Sign in using the account provided by your facilitator.
	- **Expected result:** The Microsoft Fabric home page loads.

3. **Where:** Browser (Fabric)
	- In the left navigation, select **Workspaces**.
	- **Expected result:** The Workspaces list opens.

4. **Where:** Browser (Fabric)
	- In the workspace search box, type `<<LAB_WORKSPACE_NAME>>`.
	- **Expected result:** Matching workspaces appear.

5. **Where:** Browser (Fabric)
	- Select the workspace `<<LAB_WORKSPACE_NAME>>`.
	- **Expected result:** You are in the lab workspace.

6. **Where:** Browser (Fabric)
	- In the workspace item list, select the Lakehouse `<<LAB_LAKEHOUSE_NAME>>`.
	- **Expected result:** The Lakehouse opens.

7. **Where:** Browser (Fabric)
	- Switch to the **Data Factory** experience.
	- **Expected result:** The Data Factory experience loads.

8. **Where:** Browser (Fabric)
	- In the workspace item list, select the pipeline `01_Load_PublicHolidays`.
	- **Expected result:** The pipeline details open.

9. **Where:** Browser (Fabric)
	- Open the pipeline **Output** pane.
	- **Expected result:** A recent run shows **Succeeded**.

10. **Where:** Browser (Fabric)
	- Return to the workspace item list.
	- Open the Lakehouse `<<LAB_LAKEHOUSE_NAME>>`.
	- **Expected result:** The Lakehouse opens.

11. **Where:** Browser (Fabric)
	- Under **Tables**, select `public_holidays_raw`.
	- Select **Preview**.
	- **Expected result:** You can see rows of data including `Countryorregion`.

12. **Where:** Browser (Fabric)
	- Do not create or run a notebook in this workshop.
	- **Expected result:** You avoid capacity-related compute issues when many students are working at the same time.

13. **Where:** Browser (Fabric)
	- Review the facilitator-provided output table `public_holidays_summary` in the Lakehouse.
	- **Expected result:** The table exists and is selectable under **Tables**.

14. **Where:** Browser (Fabric)
	- Select `public_holidays_summary`.
	- Select **Preview**.
	- **Expected result:** You see columns like `country_or_region` and `count`.

15. **Where:** Browser (Fabric)
	- Review the facilitator-provided audit table `agent_run_audit` in the Lakehouse.
	- **Expected result:** The table exists and is selectable under **Tables**.

16. **Where:** Browser (Fabric)
	- Select `agent_run_audit`.
	- Select **Preview**.
	- **Expected result:** You see columns like `run_timestamp_utc`, `input_table`, `output_table`, and row counts.

17. **Where:** Browser (Fabric)
	- Read the worked example below (do not run it).
	- **Expected result:** You understand what the facilitator demo did to create the output and audit tables.

	**Worked example (for reference only)**
	- `input_table`: `public_holidays_raw`
	- `output_table`: `public_holidays_summary`
	- `audit_table`: `agent_run_audit`
	- Example `run_id`: `00000000-0000-0000-0000-000000000000`

18. **Where:** Browser (Fabric)
	- Open the Lakehouse **SQL analytics endpoint** view.
	- **Expected result:** The SQL analytics endpoint opens.

19. **Where:** Browser (Fabric)
	- Select **New SQL query**.
	- **Expected result:** A new SQL query editor opens.

20. **Where:** Browser (Fabric)
	- Run the SQL query below.
	- **Expected result:** The most recent audit records are shown.

	```sql
	SELECT TOP 10
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

- The pipeline `01_Load_PublicHolidays` shows a **Succeeded** run in the pipeline **Output** pane.
- In the Lakehouse `<<LAB_LAKEHOUSE_NAME>>`, the table `public_holidays_raw` exists and previews successfully.
- In the Lakehouse, the table `public_holidays_summary` exists and previews successfully.
- In the Lakehouse, the table `agent_run_audit` exists and contains at least one row.

## Cleanup

1. **Where:** Browser
	- Close the Microsoft Fabric browser tab.
	- **Expected result:** You are signed out when the session expires.

2. **Where:** Browser (Fabric)
	- Do not delete any pipelines, notebooks, or Lakehouse tables in the shared lab workspace.
	- **Expected result:** The environment remains available for other students.

## Compliance / safety notes

- Use only synthetic or sample data in this workshop.
- Do not paste secrets, tokens, or personal data into any prompt, notebook, or pipeline.
- If you observe or participate in a notebook demo, it runs under a user identity; only run code you reviewed.

## References

- https://learn.microsoft.com/en-us/fabric/data-factory/tutorial-load-data-lakehouse-transform
- https://learn.microsoft.com/en-us/fabric/data-factory/connector-lakehouse
- https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-notebook-explore
- https://learn.microsoft.com/en-us/fabric/data-engineering/how-to-use-notebook
- https://learn.microsoft.com/en-us/fabric/data-engineering/tutorial-build-lakehouse
