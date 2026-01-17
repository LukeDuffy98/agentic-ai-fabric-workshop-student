# Lab 1 — Explore Microsoft Foundry (classic) and build an agent with knowledge + actions

## Learning objectives
- Confirm you are using Microsoft Foundry (classic) and can navigate the key areas (Agents, Model catalog, Models + endpoints, Guardrails + controls).
- Create an agent and iteratively improve it using clear instructions and policy-aligned guardrails.
- Add **Files** knowledge (file search) and validate that the agent uses uploaded policy content.
- Add a **Microsoft Fabric** knowledge tool (pre-provisioned) and validate a successful tool call.
- Add an **Azure Logic Apps** action (pre-provisioned) and validate that it writes an entity to an Azure Storage Table.

## Prereqs
- You have a web browser and you can sign in with your lab account.
- Your facilitator provides these exact values:
  - **Foundry project name:** `<<FOUNDRY_PROJECT_NAME>>`
  - **Model deployment name:** `<<MODEL_DEPLOYMENT_NAME>>`
  - **Your student ID (no names/emails):** `<<STUDENT_ID>>` (example format: `student-07`)
  - **Fabric data agent endpoint URL:** `<<FABRIC_DATA_AGENT_ENDPOINT_URL>>` (format: `https://<environment>.fabric.microsoft.com/groups/<workspace_id>/aiskills/<artifact-id>`)
  - **Logic App workflow name:** `<<LOGIC_APP_WORKFLOW_NAME>>`
  - **Storage account name:** `<<STORAGE_ACCOUNT_NAME>>`
  - **Storage table name:** `<<STORAGE_TABLE_NAME>>`

- Your facilitator has already prepared these resources:
  - A Microsoft Foundry project with at least one model deployment named `<<MODEL_DEPLOYMENT_NAME>>`.
  - A published Microsoft Fabric data agent (the lab uses its published endpoint URL).
  - An Azure Logic Apps **Consumption** workflow that starts with a **Request** trigger (with a description) and ends with a **Response** action.
  - An Azure Storage account that contains a table named `<<STORAGE_TABLE_NAME>>`.

- Required permissions (least privilege):
  - On the Foundry project: **Azure AI Developer**.
  - On the project’s storage account (for file upload tooling): **Storage Blob Data Contributor**.
  - On the Storage account (for validation in the Azure portal): **Storage Table Data Reader**.

- Local files you must have available on your computer (from this repo):
  - `labs/agentic-ai-fabric-workshop/assets/foundry/contoso-support-policy.md`
  - `labs/agentic-ai-fabric-workshop/assets/foundry/fabric_shift_constraints.md`

## Estimated time
90–120 minutes

## Architecture sketch
- A Foundry (classic) agent answers questions and uses knowledge/tools.
- File knowledge provides policy grounding from local markdown files.
- A Fabric data agent is connected as a knowledge tool for natural-language-to-SQL retrieval.
- A Logic App is connected as an action to write an audit entity to Azure Table storage.

## Step-by-step
1. (Browser) Open https://ai.azure.com/.
   - Expected result: The Microsoft Foundry portal opens.

2. (Microsoft Foundry) Set the **New Foundry** toggle to **Off**.
   - Expected result: You are in the Microsoft Foundry **(classic)** experience.

3. (Microsoft Foundry) Select your project named `<<FOUNDRY_PROJECT_NAME>>`.
   - Expected result: The project opens and the left navigation shows project pages.

4. (Microsoft Foundry) In the left navigation, select **Model catalog**.
   - Expected result: The model catalog page opens.

5. (Microsoft Foundry) In the left navigation, select **Models + endpoints**.
   - Expected result: You can see a list of deployments/endpoints for this project.

6. (Microsoft Foundry) In **Models + endpoints**, select the deployment named `<<MODEL_DEPLOYMENT_NAME>>`.
   - Expected result: The deployment details page for `<<MODEL_DEPLOYMENT_NAME>>` opens.

7. (Microsoft Foundry) Select **Edit** for the deployment.
   - Expected result: The **Update deployment** window opens and you can see the currently applied content filter configuration.

8. (Microsoft Foundry) Select **Cancel** (do not change settings).
   - Expected result: You return to the deployment details page without making changes.

9. (Microsoft Foundry) In the left navigation, select **Guardrails + controls**.
   - Expected result: The guardrails page opens.

10. (Microsoft Foundry) In **Guardrails + controls**, select the **Content filters** tab.
   - Expected result: You can see existing content filter configurations (or the option to create them).

11. (Microsoft Foundry) In **Guardrails + controls**, select the **Blocklists** tab.
   - Expected result: You can see existing blocklists (or the option to create them).

12. (Microsoft Foundry) In the left navigation, select **Agents**.
   - Expected result: The Agents page opens.

13. (Microsoft Foundry) Select **Create an agent**.
   - Expected result: The agent designer opens.

14. (Microsoft Foundry) Set these values:
   - **Name**: `Lab1-<<STUDENT_ID>>`
   - **Model deployment**: `<<MODEL_DEPLOYMENT_NAME>>`
   - Expected result: The agent shows your selected model deployment and is ready to be tested.

15. (Microsoft Foundry — Agent playground) In the agent **Instructions** box, paste the following text:
   - `You are a compliance-first assistant. Use only the information in uploaded files and tool responses. If you don't have enough information, say you don't know.`
   - Expected result: The agent instructions are updated.

16. (Microsoft Foundry — Agent playground) Ask the agent: `Confirm you are ready. Respond with only: READY`.
   - Expected result: The agent responds with `READY`.

17. (Microsoft Foundry) In the right-side **Setup** pane, under **Knowledge**, select **Add**.
   - Expected result: A list of knowledge tool options opens.

18. (Microsoft Foundry) Select **Files**.
   - Expected result: The file upload experience opens.

19. (Microsoft Foundry) Select **Select local files**, choose `labs/agentic-ai-fabric-workshop/assets/foundry/contoso-support-policy.md`, then select **Upload and save**.
   - Expected result: The file is uploaded and appears under the agent’s Knowledge section.

20. (Microsoft Foundry) Repeat the previous step to upload `labs/agentic-ai-fabric-workshop/assets/foundry/fabric_shift_constraints.md`.
   - Expected result: Both files appear under the agent’s Knowledge section.

21. (Microsoft Foundry — Agent playground) Ask the agent: `Based only on the uploaded policy, list the three most important support rules. Use a 3-bullet list.`
   - Expected result: The answer is grounded in the uploaded policy file (no invented rules).

22. (Microsoft Foundry) In the right-side **Setup** pane, under **Knowledge**, select **Add**.
   - Expected result: A list of knowledge tool options opens.

23. (Microsoft Foundry) Select **Microsoft Fabric**.
   - Expected result: The Fabric tool setup prompts appear.

24. (Microsoft Foundry) Select to add a new connection, then set:
   - `workspace-id`: Copy the value from `<<FABRIC_DATA_AGENT_ENDPOINT_URL>>` between `/groups/` and `/aiskills/`
   - `artifact-id`: Copy the value from `<<FABRIC_DATA_AGENT_ENDPOINT_URL>>` after `/aiskills/`
   - Check **Is secret** for both values
   - Expected result: The Fabric connection is created and selectable.

25. (Microsoft Foundry — Agent playground) Update the agent **Instructions** by appending this sentence:
   - `For questions about business data, use the Microsoft Fabric tool.`
   - Expected result: The instruction clearly directs when to use the Fabric tool.

26. (Microsoft Foundry — Agent playground) Ask the agent: `Use the Microsoft Fabric tool to retrieve one small factual result from the connected data agent. Then summarize it in one sentence.`
   - Expected result: The run shows a successful Fabric tool call and a summary based only on the tool response.

27. (Microsoft Foundry) In the right-side **Setup** pane, under **Actions**, select **Add**.
   - Expected result: A list of action tool options opens.

28. (Microsoft Foundry) Select **Azure Logic Apps**.
   - Expected result: The Logic Apps picker opens.

29. (Microsoft Foundry) Select the workflow named `<<LOGIC_APP_WORKFLOW_NAME>>` and finish adding it to the agent.
   - Expected result: The Logic App action appears under the agent’s Actions.

30. (Microsoft Foundry — Agent playground) Ask the agent:
   - `Call the Azure Logic Apps action to write an audit entity to Azure Table storage. Use PartitionKey "lab1" and RowKey "<<STUDENT_ID>>". Include properties "scenario"="lab1" and "status"="created".`
   - Expected result: The run shows a successful Logic App action call and a success response.

31. (Browser) Open https://portal.azure.com/.
   - Expected result: The Azure portal opens.

32. (Azure portal) Open the storage account named `<<STORAGE_ACCOUNT_NAME>>`.
   - Expected result: The Storage account overview blade opens.

33. (Azure portal) In the left navigation, select **Storage browser**.
   - Expected result: Storage browser opens.

34. (Azure portal) In Storage browser, select **Tables**, then select the table named `<<STORAGE_TABLE_NAME>>`.
   - Expected result: The table opens and you can see entities.

35. (Azure portal) Find the entity with **PartitionKey** `lab1` and **RowKey** `<<STUDENT_ID>>`.
   - Expected result: You can see the properties written by the Logic App (including `scenario` and `status`).

## Validation
- In Microsoft Foundry (classic), your agent exists and is named `Lab1-<<STUDENT_ID>>`.
- In Microsoft Foundry (classic), your agent has two uploaded files under **Knowledge** and can answer a question grounded in those files.
- In Microsoft Foundry (classic), your agent shows at least one successful **Microsoft Fabric** tool call.
- In Microsoft Foundry (classic), your agent shows a successful **Azure Logic Apps** action call.
- In the Azure portal Storage browser, the table `<<STORAGE_TABLE_NAME>>` contains an entity with PartitionKey `lab1` and RowKey `<<STUDENT_ID>>`.

## Cleanup
1. (Microsoft Foundry) In the agent designer, delete the agent `Lab1-<<STUDENT_ID>>`.
   - Expected result: The agent is removed from the Agents list.

2. (Azure portal) Close the Azure portal tab.
   - Expected result: No shared lab resources are changed.

## Compliance / safety notes
- Do not enter secrets, tokens, access keys, names, or emails in prompts.
- Use only your assigned `<<STUDENT_ID>>` as an identifier.
- Do not change model deployment settings, filters, or guardrails unless your facilitator instructs you to.

## References
- https://learn.microsoft.com/en-us/azure/ai-foundry/what-is-foundry?view=foundry-classic
- https://learn.microsoft.com/en-us/azure/ai-foundry/how-to/deploy-models-managed?view=foundry-classic#find-your-model-in-the-model-catalog
- https://learn.microsoft.com/en-us/azure/ai-foundry/openai/how-to/content-filters?view=foundry-classic#apply-a-content-filter
- https://learn.microsoft.com/en-us/azure/ai-foundry/foundry-models/how-to/use-blocklists?view=foundry-classic#create-a-blocklist
- https://learn.microsoft.com/en-us/azure/ai-foundry/agents/how-to/tools-classic/file-search-upload-files?view=foundry-classic
- https://learn.microsoft.com/en-us/azure/ai-foundry/agents/how-to/tools-classic/fabric?view=foundry-classic#setup
- https://learn.microsoft.com/en-us/azure/ai-foundry/agents/how-to/tools-classic/logic-apps?view=foundry-classic#add-a-logic-apps-workflow-to-an-agent-using-the-microsoft-foundry-portal
- https://learn.microsoft.com/en-us/azure/storage/tables/table-storage-quickstart-portal
