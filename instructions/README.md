# Agentic AI & Fabric Workshop (Portal-only)

## Learning objectives

- Create and iteratively improve an agent in Microsoft Foundry (classic).
- Add file-based knowledge and an OpenAPI tool to ground an agent’s responses.
- Use Microsoft Fabric (Data Factory + Lakehouse) to validate deterministic outputs and an audit trail in the portal.

## Prereqs

- You have access to a web browser.

- Your facilitator provides a Microsoft Fabric environment on Fabric capacity and a student account with **read access** to the lab workspace and Lakehouse.
- The facilitator provides these exact values:
  - **Workspace name:** `<<LAB_WORKSPACE_NAME>>`
  - **Lakehouse name:** `<<LAB_LAKEHOUSE_NAME>>`

- The facilitator has already prepared the Fabric demo environment (students will not run compute):
  - A pipeline named `01_Load_PublicHolidays` exists and has a recent **Succeeded** run.
  - The Lakehouse contains `public_holidays_raw`, `public_holidays_summary`, and `agent_run_audit`.

- Your facilitator provides access to Microsoft Foundry (classic) and confirms these requirements:
  - You can sign in to Microsoft Foundry.
  - The **New Foundry** toggle is set to **Off**.
  - Any required roles and permissions are already assigned to your account.

## Estimated time

- Area 1 — Agents in Foundry (classic): 90–120 minutes
- Area 2 — Fabric (portal-only): 90–120 minutes

## Architecture sketch

- In Microsoft Foundry (classic), you build an agent and improve it by refining instructions, adding knowledge files, and adding an OpenAPI tool.
- In Microsoft Fabric, a facilitator-run pipeline + notebook demo produces deterministic tables and appends an audit record.
- You validate outcomes using **Preview** and the Lakehouse **SQL analytics endpoint** (read-only).

## Step-by-step

1. (VS Code) Open the workshop exercise index at [labs/agentic-ai-fabric-workshop/starter/README.md](../starter/README.md).
   - Expected result: You can see two areas: **Agents in Foundry (classic)** and **Fabric (portal-only)**.

2. (VS Code) Complete Area 1 by following these exercises in order:
   - [labs/agentic-ai-fabric-workshop/starter/03-foundry-agent-basics/README.md](../starter/03-foundry-agent-basics/README.md)
   - [labs/agentic-ai-fabric-workshop/starter/04-foundry-knowledge-and-tools/README.md](../starter/04-foundry-knowledge-and-tools/README.md)
   - Expected result: Your agent produces a grounded, policy-compliant schedule and can call the provided OpenAPI tool.

3. (VS Code) Complete Area 2 by following these exercises in order:
   - [labs/agentic-ai-fabric-workshop/starter/01-agent-basics/README.md](../starter/01-agent-basics/README.md)
   - [labs/agentic-ai-fabric-workshop/starter/02-fabric-data-workflow/README.md](../starter/02-fabric-data-workflow/README.md)
   - [labs/agentic-ai-fabric-workshop/starter/05-fabric-audit-and-traceability/README.md](../starter/05-fabric-audit-and-traceability/README.md)
   - Expected result: You can validate the pipeline run, the Lakehouse tables, and the audit trail without running notebooks.

## Validation

- Area 1: You can demonstrate that the agent follows the uploaded policy file and uses the uploaded constraints file.
- Area 1: You can demonstrate at least one successful tool call using the OpenAPI tool.
- Area 2: You can demonstrate that `public_holidays_raw`, `public_holidays_summary`, and `agent_run_audit` can be previewed.
- Area 2: You can demonstrate that the audit table has at least one recent run record.

## Cleanup

1. (Browser) Close any Microsoft Foundry and Microsoft Fabric tabs.
   - Expected result: No shared lab resources are changed.

2. (Microsoft Fabric) Do not delete any pipelines, notebooks, or Lakehouse tables in the shared lab workspace.
   - Expected result: The environment remains available for other students.

## Compliance / safety notes

- Use only synthetic or sample data in this workshop.
- Do not paste secrets, tokens, or personal data into any prompt, notebook, or pipeline.
- Do not run notebooks in a shared classroom capacity unless your facilitator explicitly instructs you to.

## References

- https://learn.microsoft.com/en-us/azure/ai-studio/quickstarts/build-agent
- https://learn.microsoft.com/en-us/azure/ai-studio/how-to/file-search
- https://learn.microsoft.com/en-us/azure/ai-studio/how-to/tools/openapi-spec
- https://learn.microsoft.com/en-us/fabric/data-factory/tutorial-load-data-lakehouse-transform
- https://learn.microsoft.com/en-us/fabric/data-engineering/lakehouse-sql-analytics-endpoint
- https://learn.microsoft.com/en-us/fabric/data-warehouse/sql-query-editor
