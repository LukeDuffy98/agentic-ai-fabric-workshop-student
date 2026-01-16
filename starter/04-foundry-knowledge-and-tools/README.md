# Lab 2 — Add knowledge and tools to your Microsoft Foundry (classic) agent

## Learning objectives

- Add file-based knowledge to your agent using File Search.
- Ground responses in two sources:
  - A company policy file
  - A Microsoft Fabric output file (provided as a document)
- Add an OpenAPI tool and verify the agent can call it.
- Produce a final schedule that follows policy and avoids flagged days.

## Prereqs

- You completed **Lab 1** and still have your agent: `Shift-Assist-<your-initials>`.
- You have access to the same Microsoft Foundry project: `<<LAB_FOUNDRY_PROJECT_NAME>>`.
- Your lab account has the required permissions (provided by the facilitator):
  - **Azure AI Developer** role on the project
  - **Storage Blob Data Contributor** role on the project’s storage account
- You have these files available locally (in this repo):
  - `labs/agentic-ai-fabric-workshop/assets/foundry/contoso-support-policy.md`
  - `labs/agentic-ai-fabric-workshop/assets/foundry/fabric_shift_constraints.md`
  - `labs/agentic-ai-fabric-workshop/assets/foundry/openapi_worldtime_utc.json`

## Estimated time

50–60 minutes

## Architecture sketch

- Microsoft Foundry (classic) agent
- Knowledge (File Search):
  - Policy document
  - “Fabric output” document
- Tool (OpenAPI):
  - World Time API (UTC)

## Step-by-step

1. (Browser) Open https://ai.azure.com/
  - Expected result: The Microsoft Foundry portal loads.

2. (Microsoft Foundry) Set the **New Foundry** toggle to **Off**.
  - Expected result: You are in Microsoft Foundry (classic).

3. (Microsoft Foundry) Open the project `<<LAB_FOUNDRY_PROJECT_NAME>>`.
  - Expected result: The project opens.

4. (Microsoft Foundry) Select **Agents**.

5. (Microsoft Foundry) Select your agent: `Shift-Assist-<your-initials>`.
  - Expected result: The Agent playground opens.

6. (Microsoft Foundry — Agent playground) In the right-side **Setup** pane, under **Knowledge**, select **Add**.

7. (Microsoft Foundry — Agent playground) Select **Files**.

8. (Microsoft Foundry — Agent playground) Select **Select local files**.

9. (Microsoft Foundry — Agent playground) Select the file `labs/agentic-ai-fabric-workshop/assets/foundry/contoso-support-policy.md`.

10. (Microsoft Foundry — Agent playground) Select **Upload and save**.
  - Expected result: The policy file appears under Knowledge for the agent.

11. (Microsoft Foundry — Agent playground) Under **Knowledge**, select **Add** again.

12. (Microsoft Foundry — Agent playground) Select **Files**.

13. (Microsoft Foundry — Agent playground) Select **Select local files**.

14. (Microsoft Foundry — Agent playground) Select the file `labs/agentic-ai-fabric-workshop/assets/foundry/fabric_shift_constraints.md`.

15. (Microsoft Foundry — Agent playground) Select **Upload and save**.
  - Expected result: The Fabric constraints file appears under Knowledge for the agent.

16. (Microsoft Foundry — Agent playground) In the **Setup** pane, select **Instructions**.

17. (Microsoft Foundry — Agent playground) Replace the instructions with the text below.

```text
You are Shift Assist, a scheduling assistant for Contoso Support.

Goal:
- Draft a 1-week on-call schedule for a small team.

Knowledge usage rules:
- Use uploaded knowledge to answer policy and constraint questions.
- Do not guess policy or restricted dates.
- If policy/constraints are missing, ask a clarifying question.

Output rules (always, in this order):
1) Clarifying questions (0–3 bullets)
2) Proposed schedule (table)
3) Validation checks (explicitly confirm each constraint)
4) Sources (list filenames used, one per line)

Source rule:
- If you used knowledge, list it as: Source: <filename>
```

18. (Microsoft Foundry — Agent playground) Select **Save**.
  - Expected result: The agent is configured to cite sources.

19. (Microsoft Foundry — Agent playground) Send this prompt:

```text
From company policy, are there any days we must avoid scheduling on-call?
Answer with bullets and include Sources.
```

20. (Microsoft Foundry — Agent playground) Confirm the response includes:
  - A bullets answer
  - `Source: contoso-support-policy.md`
  - Expected result: The agent is demonstrably grounding on the policy file.

21. (Microsoft Foundry — Agent playground) In the **Setup** pane, scroll to **action** and select **Add**.

22. (Microsoft Foundry — Agent playground) Select **OpenAPI 3.0 specified tool**.

23. (Microsoft Foundry — Agent playground) Set:
  - **Tool name**: `worldtime_utc`
  - **Description**: `Gets the current UTC time for logging and schedule timestamps.`

24. (Microsoft Foundry — Agent playground) Select **Next**.

25. (Microsoft Foundry — Agent playground) Select **anonymous** authentication.

26. (Microsoft Foundry — Agent playground) Copy the full contents of `labs/agentic-ai-fabric-workshop/assets/foundry/openapi_worldtime_utc.json` and paste it into the OpenAPI specification box.

27. (Microsoft Foundry — Agent playground) Select **Review and add**.
  - Expected result: The OpenAPI tool appears under the agent’s action tools.

28. (Microsoft Foundry — Agent playground) Send this prompt:

```text
Call the World Time tool to get the current UTC time.
Then respond with:
- utc_time
- a one-sentence note
Include Sources (even if none).
```

29. (Microsoft Foundry — Agent playground) Confirm you see a UTC timestamp.
  - Expected result: The tool call succeeded.

30. (Microsoft Foundry — Agent playground) Send this final prompt:

```text
Create a 1-week on-call schedule for:
- Alex, Bri, Chen, Dev
Constraints:
- Each person covers exactly 2 shifts
- Each shift is 4 hours
- Week starts Monday

Extra requirements:
- Follow company policy.
- Avoid any restricted dates listed in the Fabric constraints document.
- Call the World Time tool once and include the UTC time in Notes.

Return the normal output sections, including Sources.
```

31. (Microsoft Foundry — Agent playground) Confirm the output includes:
  - A schedule table
  - Explicit validation checks
  - Sources listing both files
  - A UTC time in Notes
  - Expected result: You have a grounded schedule that uses both knowledge and a tool.

## Validation

You are successful when:

- Test prompt B cites `contoso-support-policy.md` in the Sources section.
- Test prompt C shows the tool was called and returns a UTC time.
- The Final prompt output includes:
  - A schedule table
  - Explicit validation checks
  - Sources listing:
    - `contoso-support-policy.md`
    - `fabric_shift_constraints.md`

## Cleanup

Do not delete the agent.

## Compliance / safety notes

- The “Fabric output” in this lab is a synthetic document for training.
- Do not upload real shift rosters, real incident data, or personal data.

## References

- https://learn.microsoft.com/en-us/azure/ai-foundry/agents/how-to/tools-classic/file-search-upload-files?view=foundry-classic
- https://learn.microsoft.com/en-us/azure/ai-foundry/quickstarts/get-started-code?view=foundry-classic#add-files-to-the-agent
- https://learn.microsoft.com/en-us/azure/ai-foundry/agents/how-to/tools-classic/openapi-spec-samples?view=foundry-classic
- https://learn.microsoft.com/en-us/azure/ai-foundry/what-is-azure-ai-foundry?view=foundry-classic#microsoft-foundry-portals
