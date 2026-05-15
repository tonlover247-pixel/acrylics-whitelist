# Editor Cleanup & Duplicate Resolution Design

## Overview
This design outlines the process for resolving a stale editor state where a duplicate `const supabase` declaration was causing a `SyntaxError` in an unsaved, deprecated file (`CODE.html`).

## Approach
We are proceeding with **Option 1: Manual Editor Cleanup**.

### Steps
1. **Discard Stale Buffer:** The user will manually close the `CODE.html` tab in their editor without saving.
2. **Open Production File:** The user will open the `index.html` file, which is the correct, deployed file.
3. **Verification:** The user will verify that `index.html` contains only a single `supabase` client initialization and that functions like `submitIndividual` and `updateCount` load successfully.

## Trade-offs
- Requires manual user action.
- Prevents accidental overwriting of the deployed `index.html` file with broken code from a stale buffer.

## Architecture
- No code changes are required. This is purely an environment state resolution.
