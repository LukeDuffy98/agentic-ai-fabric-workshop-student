# Exercise 01 — Agent basics in Fabric (Portal-only)

## Learning objectives

- Explain a simple agent loop as **plan → act → record**.
- Use the Lakehouse **SQL analytics endpoint** to run read-only T-SQL queries.
- Validate a deterministic result without running notebook compute.

## Prereqs

- Your facilitator provides a Microsoft Fabric workspace and Lakehouse, and you have **read access**.
- You have access to a web browser.
- You do not run notebooks in this exercise.

## Estimated time

30 minutes

## Architecture sketch

- You use the Lakehouse **SQL analytics endpoint** as a read-only query layer.
- You run a deterministic query that explicitly records the plan steps and the final result.

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
    - Expected result: The query returns a single row with `final_result = 40`.

    ```sql
    -- Agent loop example (deterministic)
    -- Task: Compute (12 * 3) + 4 and record the steps.
    SELECT
      'Compute the result of (12 * 3) + 4 and record the steps.' AS task,
      'Plan: multiply 12 by 3' AS plan_step_1,
      'Plan: add 4 to the multiplication result' AS plan_step_2,
      (12 * 3) AS step_1_result,
      ((12 * 3) + 4) AS final_result;
    ```

8. (Microsoft Fabric — Lakehouse) Run the same SQL query again.
    - Expected result: The query returns the same `final_result = 40`.

9. (Microsoft Fabric — Lakehouse) Run the SQL query below.
    - Expected result: You can see a clear separation between **plan**, **act**, and **record** in the result columns.

    ```sql
    WITH
    plan AS (
      SELECT
         'Compute (12 * 3) + 4' AS task,
         'Multiply 12 by 3' AS planned_action_1,
         'Add 4 to the result' AS planned_action_2
    ),
    act AS (
      SELECT
         (12 * 3) AS multiplication_result,
         ((12 * 3) + 4) AS final_result
    )
    SELECT
      plan.task,
      plan.planned_action_1,
      plan.planned_action_2,
      act.multiplication_result,
      act.final_result,
      'Record: store result and metadata in an audit table (facilitator-provided in later exercises)' AS record_step
    FROM plan
    CROSS JOIN act;
    ```

## Validation

- The first query returns exactly one row.
- `final_result` is `40`.
- Re-running the same query returns the same output.

## Cleanup

1. (Microsoft Fabric) Close the browser tab.
    - Expected result: No shared lab resources are changed.

## Compliance / safety notes

- Use only synthetic or sample data in this exercise.
- Do not paste secrets, tokens, or personal data into the SQL editor.

## References

- https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-sql-analytics-endpoint
- https://learn.microsoft.com/en-us/fabric/data-warehouse/sql-query-editor
