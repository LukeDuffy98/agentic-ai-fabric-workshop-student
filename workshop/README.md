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

- Download these two files to your computer (you will upload them in Foundry):
    - [assets/foundry/contoso-support-policy.md](../assets/foundry/contoso-support-policy.md)
    - [assets/foundry/fabric_shift_constraints.md](../assets/foundry/fabric_shift_constraints.md)
- For each file link, right-click the link and select **Save link as...**, then save the file to a folder you can find.

## Estimated time
90–120 minutes

## Architecture sketch
- A Foundry (classic) agent answers questions and uses knowledge/tools.
- File knowledge provides policy grounding from local markdown files.
- A Fabric data agent is connected as a knowledge tool for data-backed answers.
- A Logic App is connected as an action to write an audit entity to Azure Table storage.

## Step-by-step
1. (Browser) Open https://ai.azure.com/.

2. (Microsoft Foundry) Set the **New Foundry** toggle to **Off**.

3. (Microsoft Foundry) Select your project named `<<FOUNDRY_PROJECT_NAME>>`.

4. (Microsoft Foundry) Select **Model catalog**.

5. (Microsoft Foundry) Select **Models + endpoints**.

6. (Microsoft Foundry) Select the deployment named `<<MODEL_DEPLOYMENT_NAME>>`.

7. (Microsoft Foundry) Select **Edit**.

8. (Microsoft Foundry) Select **Cancel**.

9. (Microsoft Foundry) Select **Guardrails + controls**.

10. (Microsoft Foundry) Select the **Content filters** tab.

11. (Microsoft Foundry) Select the **Blocklists** tab.

12. (Microsoft Foundry) Select **Agents**.

13. (Microsoft Foundry) Select **Create an agent**.

14. (Microsoft Foundry) In **Name**, enter `Lab1-<<STUDENT_ID>>`.

15. (Microsoft Foundry) In **Model deployment**, select `<<MODEL_DEPLOYMENT_NAME>>`.

16. (Microsoft Foundry) Select **Create**.

17. (Microsoft Foundry — Agent playground) In **Instructions**, paste this text:

    `You are a compliance-first assistant. Use only the information in uploaded files and tool responses. If you don't have enough information, say you don't know.`

18. (Microsoft Foundry — Agent playground) In the chat box, send: `Confirm you are ready. Respond with only: READY`.

19. (Microsoft Foundry — Agent playground) In the right-side **Setup** pane, under **knowledge**, select **Add**.

20. (Microsoft Foundry — Agent playground) Select **Files**.

21. (Microsoft Foundry — Agent playground) Select **Select local files**.

22. (Microsoft Foundry — Agent playground) Select the file you downloaded named `contoso-support-policy.md`.

23. (Microsoft Foundry — Agent playground) Select **Upload and save**.

24. (Microsoft Foundry — Agent playground) Select **Select local files**.

25. (Microsoft Foundry — Agent playground) Select the file you downloaded named `fabric_shift_constraints.md`.

26. (Microsoft Foundry — Agent playground) Select **Upload and save**.

27. (Microsoft Foundry — Agent playground) In the chat box, send: `Based only on the uploaded policy, list the three most important support rules. Use a 3-bullet list.`

28. (Microsoft Foundry — Agent playground) Under **knowledge**, select **Add**.

29. (Microsoft Foundry — Agent playground) Select **Microsoft Fabric**.

30. (Microsoft Foundry — Agent playground) Select **Add connection**.

31. (Microsoft Foundry — Agent playground) In `workspace-id`, paste the value from `<<FABRIC_DATA_AGENT_ENDPOINT_URL>>` between `/groups/` and `/aiskills/`.

32. (Microsoft Foundry — Agent playground) For `workspace-id`, select **is secret**.

33. (Microsoft Foundry — Agent playground) In `artifact-id`, paste the value from `<<FABRIC_DATA_AGENT_ENDPOINT_URL>>` after `/aiskills/`.

34. (Microsoft Foundry — Agent playground) For `artifact-id`, select **is secret**.

35. (Microsoft Foundry — Agent playground) Select **Save**.

36. (Microsoft Foundry — Agent playground) In **Instructions**, append this sentence:

    `For questions about business data, use the Microsoft Fabric tool.`

37. (Microsoft Foundry — Agent playground) In the chat box, send: `Use the Microsoft Fabric tool to retrieve one small factual result from the connected data agent. Then summarize it in one sentence.`

38. (Microsoft Foundry — Agent playground) Under **Actions**, select **Add**.

39. (Microsoft Foundry — Agent playground) Select **Azure Logic Apps**.

40. (Microsoft Foundry — Agent playground) Select the workflow named `<<LOGIC_APP_WORKFLOW_NAME>>`.

41. (Microsoft Foundry — Agent playground) Select **Add**.

42. (Microsoft Foundry — Agent playground) In the chat box, send:

    `Call the Azure Logic Apps action to write an audit entity to Azure Table storage. Use PartitionKey "lab1" and RowKey "<<STUDENT_ID>>". Include properties "scenario"="lab1" and "status"="created".`

43. (Browser) Open https://portal.azure.com/.

44. (Azure portal) Open the storage account named `<<STORAGE_ACCOUNT_NAME>>`.

45. (Azure portal) Select **Storage Browser**.

46. (Azure portal) Select **Tables**.

47. (Azure portal) Select the table named `<<STORAGE_TABLE_NAME>>`.

48. (Azure portal) Find the entity with **PartitionKey** `lab1`.

49. (Azure portal) Confirm the entity has **RowKey** `<<STUDENT_ID>>`.

## Validation
- In Microsoft Foundry (classic), your agent exists and is named `Lab1-<<STUDENT_ID>>`.
- In Microsoft Foundry (classic), your agent has two uploaded files under **knowledge** and can answer a question grounded in those files.
- In Microsoft Foundry (classic), your agent run history shows at least one successful **Microsoft Fabric** tool call.
- In Microsoft Foundry (classic), your agent run history shows a successful **Azure Logic Apps** action call.
- In the Azure portal Storage Browser, the table `<<STORAGE_TABLE_NAME>>` contains an entity with PartitionKey `lab1` and RowKey `<<STUDENT_ID>>`.

## Cleanup
1. (Microsoft Foundry) Select **Agents**.

2. (Microsoft Foundry) Select the agent `Lab1-<<STUDENT_ID>>`.

3. (Microsoft Foundry) Select **Delete**.

4. (Microsoft Foundry) Select **Delete**.

5. (Browser) Close the Azure portal tab.

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
