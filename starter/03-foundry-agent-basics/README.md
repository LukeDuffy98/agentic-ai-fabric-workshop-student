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

50–60 minutes

## Architecture sketch

- Microsoft Foundry (classic) project
- One agent (you will keep and reuse this same agent in Lab 2)
- Agent playground for testing prompts and reviewing responses

## Step-by-step

1. (Browser) Open https://ai.azure.com/
  - Expected result: The Microsoft Foundry sign-in page appears.

2. (Browser) Sign in with the lab account provided by your facilitator.
  - Expected result: The Microsoft Foundry portal loads.

3. (Microsoft Foundry) In the top-right area, find the **New Foundry** toggle.

4. (Microsoft Foundry) Set **New Foundry** to **Off**.
  - Expected result: You are in Microsoft Foundry (classic).

5. (Microsoft Foundry) Open the project named `<<LAB_FOUNDRY_PROJECT_NAME>>`.
  - Expected result: You can see project experiences such as **Agents**.

6. (Microsoft Foundry) Select **Agents**.
  - Expected result: You see the Agents screen.

7. (Microsoft Foundry) Select **Create an agent**.
  - Expected result: The agent create screen opens.

8. (Microsoft Foundry) In the create screen, set:
  - **Name**: `Shift-Assist-<your-initials>`
  - **Model**: `<<LAB_MODEL_DEPLOYMENT_NAME>>`

9. (Microsoft Foundry) Select **Create**.
  - Expected result: Your agent opens in the **Agent playground** with a right-side **Setup** pane.

10. (Microsoft Foundry — Agent playground) In the **Setup** pane, select **Instructions**.

11. (Microsoft Foundry — Agent playground) Replace the instructions with the text below.

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

12. (Microsoft Foundry — Agent playground) Select **Save**.
  - Expected result: The agent instructions are saved.

13. (Microsoft Foundry — Agent playground) Send this prompt:

```text
Create a 1-week on-call schedule for:
- Alex, Bri, Chen, Dev
Constraints:
- Each person covers exactly 2 shifts
- Each shift is 4 hours
- Week starts Monday
Ask up to 3 clarifying questions if needed.
```

14. (Microsoft Foundry — Agent playground) Confirm the agent returns:
  - A schedule table
  - Notes
  - No more than 3 clarifying questions
  - Expected result: You have a usable “first draft” schedule.

15. (Microsoft Foundry — Agent playground) In the **Setup** pane, select **Instructions**.

16. (Microsoft Foundry — Agent playground) Replace the instructions with the text below.

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
- Include bullets that explicitly confirm each stated constraint.

Knowledge citation:
- If you used any uploaded knowledge, cite it as: Source: <filename>
```

17. (Microsoft Foundry — Agent playground) Select **Save**.
  - Expected result: The updated instructions are saved.

18. (Microsoft Foundry — Agent playground) Re-run the same schedule prompt from step 13.
  - Expected result: The response now includes a “Validation checks” section.

19. (Microsoft Foundry — Agent playground) Send this “missing requirements” prompt:

```text
Draft a 1-week on-call schedule for Alex, Bri, Chen, Dev.
```

20. (Microsoft Foundry — Agent playground) Confirm the agent asks clarifying questions instead of guessing.
  - Expected result: You see up to 3 clarifying questions.

21. (Microsoft Foundry — Agent playground) Send this “conflicting constraints” prompt:

```text
Create a 1-week on-call schedule for Alex, Bri, Chen, Dev.
Constraints:
- Each person covers exactly 10 shifts
- Week has only 10 total shifts
```

22. (Microsoft Foundry — Agent playground) Confirm the agent proposes the smallest change needed.
  - Expected result: The agent explains why the constraints cannot both be true.

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
