# Lab 1 — Build your first agent in Microsoft Foundry (classic)

## Learning objectives

- Open Microsoft Foundry (classic) and access the Agent playground.
- Create a basic agent and validate responses with repeatable test prompts.
- Improve agent instructions to reduce hallucinations and increase consistency.
- Establish a single, continuous scenario that continues into Lab 2.

## Prereqs

- You have a lab account that can access a Microsoft Foundry project.
- A facilitator provides:
  - The Microsoft Foundry project to use: `<<LAB_FOUNDRY_PROJECT_NAME>>`
  - The model deployment to use in that project: `<<LAB_MODEL_DEPLOYMENT_NAME>>`
- You are signed in with your lab account.

## Estimated time

30–40 minutes

## Architecture sketch

- Microsoft Foundry (classic) project
- One agent (you will keep and reuse this same agent in Lab 2)
- Agent playground for testing prompts and reviewing responses

## Step-by-step

### 1) Open Microsoft Foundry (classic)

**Where you are:** Web browser

1. Go to `https://ai.azure.com/`.
2. In the top-right area of the portal, find the **New Foundry** toggle.
3. Set **New Foundry** to **Off**.
4. Confirm you are in the **classic** experience.

**Expected result:** You are in Microsoft Foundry (classic).

### 2) Open the lab project

**Where you are:** Microsoft Foundry (classic)

1. In the Microsoft Foundry home experience, open the project named `<<LAB_FOUNDRY_PROJECT_NAME>>`.

**Expected result:** You are inside the project and can see project experiences such as Agents.

### 3) Create your agent

**Where you are:** Microsoft Foundry (classic)

1. Open **Agents**.
2. Select **Create an agent**.
3. In the create screen, set:
   - **Name**: `Shift-Assist-<your-initials>`
   - **Model**: select `<<LAB_MODEL_DEPLOYMENT_NAME>>`
4. Select **Create**.

**Expected result:** Your agent opens in the **Agent playground** (or a create/debug screen) with a right-side **Setup** pane.

### 4) Add baseline instructions (Version 1)

**Where you are:** Agent playground

1. In the right-side **Setup** pane, find **Instructions**.
2. Replace the instructions with the text below.
3. Select **Save** (or **Update**, depending on what the portal shows).

**Instructions (V1)**

```text
You are Shift Assist, a scheduling assistant for Contoso Support.

Your job:
- Help a team lead draft a 1-week on-call schedule for a small team.

Hard rules:
- Do not invent company policies. If you do not know, ask a question.
- When you use facts from knowledge sources, say which file you used.

Response format (always):
1) Clarifying questions (0–3 bullets)
2) Proposed schedule (table)
3) Notes (short bullets)
```

**Expected result:** The agent instructions are saved.

### 5) Test the agent with a repeatable prompt

**Where you are:** Agent playground

1. In the chat area, enter the prompt below.
2. Send the prompt.

**Test prompt A**

```text
Create a 1-week on-call schedule for:
- Alex, Bri, Chen, Dev
Constraints:
- Each person covers exactly 2 shifts
- Each shift is 4 hours
- Week starts Monday
Ask up to 3 clarifying questions if needed.
```

**Expected result:** You get a schedule table and short notes.

### 6) Improve instructions to reduce randomness (Version 2)

**Where you are:** Agent playground

1. In **Setup** → **Instructions**, replace the instructions with the text below.
2. Select **Save**.

**Instructions (V2)**

```text
You are Shift Assist, a scheduling assistant for Contoso Support.

Goal:
- Draft a 1-week on-call schedule for a small team.

Operating rules:
- If a requirement is missing, ask clarifying questions first (max 3).
- If you cannot satisfy all constraints, propose the smallest change needed and explain why.
- Do not invent policies, dates, or external facts.

Output rules:
- Always output these sections, in this order:
  1) Clarifying questions
  2) Proposed schedule
  3) Validation checks
  4) Notes

Validation checks:
- Include a bullet list that explicitly confirms each stated constraint.

Knowledge citation:
- If you used any uploaded knowledge, cite it as: Source: <filename>
```

**Expected result:** The updated instructions are saved.

### 7) Re-test and compare

**Where you are:** Agent playground

1. Re-run **Test prompt A**.
2. Check whether the agent:
   - Asks fewer unnecessary questions
   - Adds a “Validation checks” section
   - Clearly confirms each constraint

**Expected result:** The response is more structured and self-checking than before.

## Validation

You are successful when:

- Your agent exists and is named `Shift-Assist-<your-initials>`.
- The response to **Test prompt A** includes all required sections.
- The response includes a clear “Validation checks” section that confirms:
  - The team list
  - Exactly 2 shifts per person
  - 4-hour shifts
  - Week starts Monday

## Cleanup

Do not delete the agent. You will reuse it in Lab 2.

## Compliance / safety notes

- Use synthetic names only (Alex/Bri/Chen/Dev).
- Do not paste real employee schedules or personal data.

## References

- https://learn.microsoft.com/en-us/azure/ai-foundry/agents/quickstart?view=foundry-classic
- https://learn.microsoft.com/en-us/azure/ai-foundry/quickstarts/get-started-code?view=foundry-classic
- https://learn.microsoft.com/en-us/azure/ai-foundry/what-is-azure-ai-foundry?view=foundry-classic#microsoft-foundry-portals
