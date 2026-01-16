# Agentic AI & Fabric Workshop

## Learning objectives

- Understand what “agentic AI” means in practical terms (tools, memory, orchestration).
- Use Microsoft Fabric as the data and analytics foundation for an AI solution.
- Build a simple end-to-end workflow that reads data, reasons over it, and produces an auditable output.

## Prereqs

- A facilitator-provided Microsoft Fabric environment (capacity + a workspace you can access).
- A facilitator-provided student account and temporary password.
- A facilitator-provided set of lab resources (workspace name, dataset/lakehouse name).
- You have access to a web browser and VS Code.

## Estimated time

- 60–90 minutes

## Architecture sketch

- Fabric workspace hosts the data artifacts used in the exercises.
- Your agent/workflow reads from Fabric, produces an output, and records what it did.
- Outputs are stored in a lab folder for review and validation.

## Step-by-step

1. **Where:** Browser
	- Sign in to the facilitator-provided Microsoft Fabric environment.
	- **Expected result:** You can see the provided Fabric workspace.

2. **Where:** VS Code
	- Open the workshop repo you were given (student repo).
	- **Expected result:** You can see `README.md`, `labs/`, and `docs/` at the repo root.

3. **Where:** Browser (Fabric)
	- Open the provided lab workspace.
	- Confirm you can access the provided dataset/lakehouse.
	- **Expected result:** The dataset/lakehouse opens without access errors.

4. **Where:** VS Code
	- Complete Exercise 1 in `starter/01-agent-basics/README.md`.
	- **Expected result:** You generate a deterministic output file in the location specified by the exercise.

5. **Where:** VS Code
	- Complete Exercise 2 in `starter/02-fabric-data-workflow/README.md`.
	- **Expected result:** You can trace which input data was used and what output was produced.

## Validation

- You can access the facilitator-provided Fabric workspace.
- You can complete each exercise and produce the expected output files.
- Your outputs include timestamps and clearly reference the input data used.

## Cleanup

1. **Where:** VS Code
	- Delete any local `out/` folder created by the exercises.
	- **Expected result:** Your working directory no longer contains generated outputs.

## Compliance / safety notes

- Use only synthetic sample data in this workshop.
- Do not paste secrets, tokens, or personal data into prompts or files.
- Keep least-privilege access: do not grant access outside the lab workspace.
