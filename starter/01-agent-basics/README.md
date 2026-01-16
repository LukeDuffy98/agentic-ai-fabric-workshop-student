# Exercise 01 — Agent basics (Portal-only)

## Goal

Review a small, deterministic “agent loop” (plan → act → record) without using notebook compute.

## What you will do

- Run a deterministic SQL query that represents the plan and the act steps.
- Validate the output value is correct.

## Instructions

1. **Where:** Browser (Fabric)
    - Open `https://app.fabric.microsoft.com`.
    - Open the workspace provided by your facilitator.
    - Open the Lakehouse provided by your facilitator.
    - **Expected result:** You can see **Tables** and **Files** for the Lakehouse.

2. **Where:** Browser (Fabric)
    - Open the Lakehouse **SQL analytics endpoint** view.
    - **Expected result:** The SQL analytics endpoint opens.

3. **Where:** Browser (Fabric)
    - Select **New SQL query**.
    - **Expected result:** A new SQL query editor opens.

4. **Where:** Browser (Fabric)
    - Run the SQL query below.
    - **Expected result:** The query returns `final_result = 40`.

    ```sql
    -- Task: Compute (12 * 3) + 4 and record the steps.
    -- This is a deterministic, capacity-friendly “agent loop” example.
    SELECT
      'Compute the result of (12 * 3) + 4 and record the steps.' AS task,
      'Use multiply(12, 3)' AS plan_step_1,
      'Use add(<step_1_result>, 4)' AS plan_step_2,
      (12 * 3) AS step_1_result,
      ((12 * 3) + 4) AS final_result;
    ```

## Validation

- The SQL query returns a single row.
- `final_result` is `40`.

## Cleanup

No cleanup is required for this exercise.
