# Lab 1 — Explore Microsoft Foundry (classic) and build an agent with knowledge + actions

## Learning objectives
- Confirm you are using Microsoft Foundry (classic) and can navigate the key areas.
- Create an agent and iteratively improve it using clear instructions.
- Add **Files** knowledge and validate grounded answers from uploaded policy content.
- Add a pre-provisioned **Microsoft Fabric** data agent connection and validate a successful tool call.
- Add a pre-provisioned **Azure Logic Apps** action and validate that it writes an entity to an Azure Storage Table.

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

- Your facilitator has already prepared these resources (students do not create Azure resources in this lab):
  - A Microsoft Foundry project with a model deployment named `<<MODEL_DEPLOYMENT_NAME>>`.
  - A published Microsoft Fabric data agent (you will use its published endpoint URL).
  - An Azure Logic Apps **Consumption** workflow that:
    - Is in the same subscription and resource group as the Foundry project.
    - Starts with a **Request** trigger (with a description).
    - Ends with a **Response** action.
    - Writes an entity to the table `<<STORAGE_TABLE_NAME>>` in storage account `<<STORAGE_ACCOUNT_NAME>>`.
  - An Azure Storage account that contains a table named `<<STORAGE_TABLE_NAME>>`.

- Required permissions (least privilege):
  - On the Foundry project: **Azure AI Developer**.
  - On the project’s storage account (for file uploads): **Storage Blob Data Contributor**.
  - On the storage account (for validation in the Azure portal): **Storage Table Data Reader**.

- Local files you must have available on your computer (from the student repo):
  - `assets/foundry/contoso-support-policy.md`
  - `assets/foundry/fabric_shift_constraints.md`

## Estimated time
90–120 minutes

## Architecture sketch
- A Foundry (classic) agent answers questions and uses knowledge/tools.
- File knowledge provides policy grounding from local markdown files.
- A Fabric data agent is connected as a knowledge tool for data-backed answers.
- A Logic App is connected as an action to write an audit entity to Azure Table storage.

## Step-by-step
1. (Browser) Open https://ai.azure.com/.
   - Expected result: The Microsoft Foundry portal opens.

2. (Microsoft Foundry) Set the **New Foundry** toggle to **Off**.
   - Expected result: You are in the Microsoft Foundry **(classic)** experience.

3. (Microsoft Foundry) Select your project named `<<FOUNDRY_PROJECT_NAME>>`.
   - Expected result: The project opens.

4. (Microsoft Foundry) Select **Model catalog**.
   - Expected result: The model catalog page opens.

5. (Microsoft Foundry) Select **Models + endpoints**.
   - Expected result: You can see a list of deployments/endpoints for this project.

6. (Microsoft Foundry) Select the deployment named `<<MODEL_DEPLOYMENT_NAME>>`.
   - Expected result: The deployment details page opens.

7. (Microsoft Foundry) Select **Edit**.
   - Expected result: The deployment update window opens.

8. (Microsoft Foundry) Select **Cancel**.
   - Expected result: You return to the deployment details page without making changes.

9. (Microsoft Foundry) Select **Guardrails + controls**.
   - Expected result: The guardrails page opens.

10. (Microsoft Foundry) Select the **Content filters** tab.
    - Expected result: You can see existing content filter configurations (or the option to create them).

11. (Microsoft Foundry) Select the **Blocklists** tab.
    - Expected result: You can see existing blocklists (or the option to create them).

12. (Microsoft Foundry) Select **Agents**.
    - Expected result: The Agents page opens.

13. (Microsoft Foundry) Select **Create an agent**.
    - Expected result: The agent designer opens.

14. (Microsoft Foundry) In **Name**, enter `Lab1-<<STUDENT_ID>>`.
    - Expected result: The name field contains `Lab1-<<STUDENT_ID>>`.

15. (Microsoft Foundry) In **Model deployment**, select `<<MODEL_DEPLOYMENT_NAME>>`.
    - Expected result: The selected model deployment shows as `<<MODEL_DEPLOYMENT_NAME>>`.

16. (Microsoft Foundry) Select **Create**.
    - Expected result: The agent is created and the agent playground is available.

17. (Microsoft Foundry — Agent playground) In **Instructions**, paste this text:

    `You are a compliance-first assistant. Use only the information in uploaded files and tool responses. If you don't have enough information, say you don't know.`

    - Expected result: The instructions are updated.

18. (Microsoft Foundry — Agent playground) In the chat box, send: `Confirm you are ready. Respond with only: READY`.
    - Expected result: The agent responds with `READY`.

19. (Microsoft Foundry — Agent playground) In the right-side **Setup** pane, under **knowledge**, select **Add**.
    - Expected result: A list of knowledge tool options opens.

20. (Microsoft Foundry — Agent playground) Select **Files**.
    - Expected result: The file upload page opens.

21. (Microsoft Foundry — Agent playground) Select **Select local files**.
    - Expected result: A file picker opens.

22. (Microsoft Foundry — Agent playground) Select the file `assets/foundry/contoso-support-policy.md`.
    - Expected result: The file is selected in the upload UI.

23. (Microsoft Foundry — Agent playground) Select **Upload and save**.
    - Expected result: The file upload completes and the file appears under the agent’s knowledge.

24. (Microsoft Foundry — Agent playground) Select **Select local files**.
    - Expected result: A file picker opens.

25. (Microsoft Foundry — Agent playground) Select the file `assets/foundry/fabric_shift_constraints.md`.
    - Expected result: The file is selected in the upload UI.

26. (Microsoft Foundry — Agent playground) Select **Upload and save**.
    - Expected result: The file upload completes and both files appear under the agent’s knowledge.

27. (Microsoft Foundry — Agent playground) In the chat box, send: `Based only on the uploaded policy, list the three most important support rules. Use a 3-bullet list.`
    - Expected result: The answer is grounded in the uploaded files (no invented rules).

28. (Microsoft Foundry — Agent playground) Under **knowledge**, select **Add**.
    - Expected result: A list of knowledge tool options opens.

29. (Microsoft Foundry — Agent playground) Select **Microsoft Fabric**.
    - Expected result: The Fabric tool setup prompts appear.

30. (Microsoft Foundry — Agent playground) Select **Add connection**.
    - Expected result: A connection form opens.

31. (Microsoft Foundry — Agent playground) In `workspace-id`, paste the value from `<<FABRIC_DATA_AGENT_ENDPOINT_URL>>` between `/groups/` and `/aiskills/`.
    - Expected result: The `workspace-id` field is populated.

32. (Microsoft Foundry — Agent playground) For `workspace-id`, select **is secret**.
    - Expected result: `workspace-id` is marked as secret.

33. (Microsoft Foundry — Agent playground) In `artifact-id`, paste the value from `<<FABRIC_DATA_AGENT_ENDPOINT_URL>>` after `/aiskills/`.
    - Expected result: The `artifact-id` field is populated.

34. (Microsoft Foundry — Agent playground) For `artifact-id`, select **is secret**.
    - Expected result: `artifact-id` is marked as secret.

35. (Microsoft Foundry — Agent playground) Select **Save**.
    - Expected result: The Fabric connection is created and selected for the agent.

36. (Microsoft Foundry — Agent playground) In **Instructions**, append this sentence:

    `For questions about business data, use the Microsoft Fabric tool.`

    - Expected result: The instructions clearly direct when to use the Fabric tool.

37. (Microsoft Foundry — Agent playground) In the chat box, send: `Use the Microsoft Fabric tool to retrieve one small factual result from the connected data agent. Then summarize it in one sentence.`
    - Expected result: The run shows a successful Microsoft Fabric tool call and a summary based only on the tool response.

38. (Microsoft Foundry — Agent playground) Under **Actions**, select **Add**.
    - Expected result: A list of action tool options opens.

39. (Microsoft Foundry — Agent playground) Select **Azure Logic Apps**.
    - Expected result: The Logic Apps picker opens.

40. (Microsoft Foundry — Agent playground) Select the workflow named `<<LOGIC_APP_WORKFLOW_NAME>>`.
    - Expected result: The selected workflow is ready to be added.

41. (Microsoft Foundry — Agent playground) Select **Add**.
    - Expected result: The Logic App workflow appears under the agent’s Actions.

42. (Microsoft Foundry — Agent playground) In the chat box, send:

    `Call the Azure Logic Apps action to write an audit entity to Azure Table storage. Use PartitionKey "lab1" and RowKey "<<STUDENT_ID>>". Include properties "scenario"="lab1" and "status"="created".`

    - Expected result: The run shows a successful Azure Logic Apps action call.

43. (Browser) Open https://portal.azure.com/.
    - Expected result: The Azure portal opens.

44. (Azure portal) Open the storage account named `<<STORAGE_ACCOUNT_NAME>>`.
    - Expected result: The Storage account overview page opens.

45. (Azure portal) Select **Storage Browser**.
    - Expected result: Storage Browser opens.

46. (Azure portal) Select **Tables**.
    - Expected result: The Tables list opens.

47. (Azure portal) Select the table named `<<STORAGE_TABLE_NAME>>`.
    - Expected result: The table opens and you can see entities.

48. (Azure portal) Find the entity with **PartitionKey** `lab1`.
    - Expected result: At least one matching entity is visible.

49. (Azure portal) Confirm the entity has **RowKey** `<<STUDENT_ID>>`.
    - Expected result: You can see properties written by the Logic App (including `scenario` and `status`).

## Validation
- In Microsoft Foundry (classic), your agent exists and is named `Lab1-<<STUDENT_ID>>`.
- In Microsoft Foundry (classic), your agent has two uploaded files under **knowledge** and can answer a question grounded in those files.
- In Microsoft Foundry (classic), your agent run history shows at least one successful **Microsoft Fabric** tool call.
- In Microsoft Foundry (classic), your agent run history shows a successful **Azure Logic Apps** action call.
- In the Azure portal Storage Browser, the table `<<STORAGE_TABLE_NAME>>` contains an entity with PartitionKey `lab1` and RowKey `<<STUDENT_ID>>`.

## Cleanup
1. (Microsoft Foundry) Select **Agents**.
   - Expected result: The Agents page opens.

2. (Microsoft Foundry) Select the agent `Lab1-<<STUDENT_ID>>`.
   - Expected result: The agent details page opens.

3. (Microsoft Foundry) Select **Delete**.
   - Expected result: A delete confirmation prompt appears.

4. (Microsoft Foundry) Select **Delete**.
   - Expected result: The agent is removed from the Agents list.

5. (Browser) Close the Azure portal tab.
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
