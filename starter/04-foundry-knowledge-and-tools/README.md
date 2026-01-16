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

40–55 minutes

## Architecture sketch

- Microsoft Foundry (classic) agent
- Knowledge (File Search):
  - Policy document
  - “Fabric output” document
- Tool (OpenAPI):
  - World Time API (UTC)

## Step-by-step

### 1) Open Microsoft Foundry (classic) and your agent

**Where you are:** Web browser

1. Go to `https://ai.azure.com/`.
2. Set the **New Foundry** toggle to **Off**.
3. Open the project `<<LAB_FOUNDRY_PROJECT_NAME>>`.
4. Open **Agents**.
5. Select your agent: `Shift-Assist-<your-initials>`.

**Expected result:** The Agent playground opens for your existing agent.

### 2) Add knowledge (File Search) — company policy

**Where you are:** Agent playground

1. In the right-side **Setup** pane, scroll to **Knowledge**.
2. Select **Add**.
3. Select **Files**.
4. Select **Select local files**.
5. Choose `labs/agentic-ai-fabric-workshop/assets/foundry/contoso-support-policy.md`.
6. Select **Upload and save**.

**Expected result:** The file is added under the agent’s knowledge.

### 3) Add knowledge (File Search) — “Fabric output” constraints

**Where you are:** Agent playground

1. In **Setup** → **Knowledge**, select **Add**.
2. Select **Files**.
3. Select **Select local files**.
4. Choose `labs/agentic-ai-fabric-workshop/assets/foundry/fabric_shift_constraints.md`.
5. Select **Upload and save**.

**Expected result:** The “Fabric output” document is added under the agent’s knowledge.

### 4) Update instructions to require grounded answers (Version 3)

**Where you are:** Agent playground

1. In **Setup** → **Instructions**, replace the instructions with the text below.
2. Select **Save**.

**Instructions (V3)**

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

**Expected result:** Your agent is configured to cite the uploaded knowledge.

### 5) Validate the agent is using knowledge

**Where you are:** Agent playground

1. Send the prompt below.

**Test prompt B**

```text
From company policy, are there any days we must avoid scheduling on-call?
Answer with bullets and include Sources.
```

**Expected result:** The agent answers and includes a **Sources** section listing `contoso-support-policy.md`.

### 6) Add an OpenAPI tool (World Time API)

**Where you are:** Agent playground

1. In the right-side **Setup** pane, scroll to **action**.
2. Select **Add**.
3. Select **OpenAPI 3.0 specified tool**.
4. Set:
   - **Tool name**: `worldtime_utc`
   - **Description**: `Gets the current UTC time for logging and schedule timestamps.`
5. Select **Next**.
6. In the authentication step, select **anonymous**.
7. Copy the full contents of `labs/agentic-ai-fabric-workshop/assets/foundry/openapi_worldtime_utc.json` and paste it into the OpenAPI specification box.
8. Select **Review and add** (or **Add tool**, depending on what the portal shows).

**Expected result:** The OpenAPI tool appears under the agent’s **action** tools.

### 7) Prove the tool works

**Where you are:** Agent playground

1. Send the prompt below.

**Test prompt C**

```text
Call the World Time tool to get the current UTC time.
Then respond with:
- utc_time
- a one-sentence note
Include Sources (even if none).
```

**Expected result:** The agent calls the tool and returns a UTC timestamp.

### 8) Final scenario prompt — policy + Fabric constraints + tool

**Where you are:** Agent playground

1. Send the prompt below.

**Final prompt**

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

**Expected result:** The agent produces a schedule, validates constraints, cites both files, and includes UTC time in Notes.

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
