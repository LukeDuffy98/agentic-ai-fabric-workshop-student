# Exercise 02 — Fabric data workflow (Portal-only)

## Goal

Use Microsoft Fabric to:

1. Review a pipeline run that loaded sample data into a Lakehouse table.
2. Validate the loaded data.
3. Validate the facilitator-provided output and audit tables.

## Instructions

1. **Where:** Browser (Fabric)
   - Open `https://app.fabric.microsoft.com`.
   - Open the workspace provided by your facilitator.
   - **Expected result:** The workspace item list is visible.

2. **Where:** Browser (Fabric)
   - Switch to the **Data Factory** experience.
   - **Expected result:** The Data Factory experience loads.

3. **Where:** Browser (Fabric)
   - In the workspace item list, select the pipeline `01_Load_PublicHolidays`.
   - **Expected result:** The pipeline details open.

4. **Where:** Browser (Fabric)
   - Open the pipeline **Output** pane.
   - **Expected result:** A recent run shows **Succeeded**.

5. **Where:** Browser (Fabric)
   - Return to the workspace item list.
   - Open the Lakehouse provided by your facilitator.
   - **Expected result:** The Lakehouse opens.

6. **Where:** Browser (Fabric)
   - Under **Tables**, select `public_holidays_raw`.
   - Select **Preview**.
   - **Expected result:** You can see rows and a column named `Countryorregion`.

7. **Where:** Browser (Fabric)
   - Under **Tables**, select `public_holidays_summary`.
   - Select **Preview**.
   - **Expected result:** You can see a summary of countries/regions and counts.

8. **Where:** Browser (Fabric)
   - Under **Tables**, select `agent_run_audit`.
   - Select **Preview**.
   - **Expected result:** You can see audit rows including `input_table` and `output_table`.

## Validation

- `public_holidays_raw` exists and previews successfully.
- `public_holidays_summary` exists and previews successfully.
- `agent_run_audit` exists and contains at least one row.

## Cleanup

No cleanup is required for this exercise. Do not delete shared lab resources.
