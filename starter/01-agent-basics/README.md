# Exercise 01 — Agent basics

## Goal

Create a small, auditable “agent loop” that:

1. Takes a simple task description.
2. Chooses from a fixed set of tools.
3. Produces a deterministic output file.

## What you will do

- Read the provided prompt in `prompt.txt`.
- Produce an output file `out/exercise-01-result.md`.

## Instructions

1. **Where:** VS Code
   - Open this folder.
   - Open `prompt.txt`.
   - **Expected result:** You understand the task to complete.

2. **Where:** VS Code / terminal
   - Create an `out/` folder at the repo root.
   - Create `out/exercise-01-result.md`.
   - **Expected result:** The file exists and is not empty.

## Validation

- `out/exercise-01-result.md` exists.
- It includes:
  - A timestamp
  - The input prompt text you used
  - The final answer

## Cleanup

Delete the `out/` folder when done.
