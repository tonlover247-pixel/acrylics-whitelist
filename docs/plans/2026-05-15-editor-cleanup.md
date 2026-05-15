# Editor Cleanup Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Resolve a stale editor buffer causing duplicate `supabase` initialization errors.

**Architecture:** A manual workspace reset focusing on the user's local code editor environment.

**Tech Stack:** Local Code Editor (VS Code / Cursor / etc.)

---

### Task 1: Close Stale Buffer

**Files:**
- Modify: `CODE.html` (in editor memory)

**Step 1: Close `CODE.html` without saving**
- User Action: Click the "X" on the `CODE.html` tab in the editor.
- When prompted to save, select "Don't Save" to discard the stale memory buffer.

**Step 2: Verify it is gone**
- Ensure `CODE.html` no longer appears in the editor tab bar.

### Task 2: Open and Verify Correct File

**Files:**
- View: `index.html`

**Step 1: Open the deployed file**
- User Action: Open the `index.html` file from the `ACRYLICS` folder.

**Step 2: Verify `index.html` content**
- Scroll to line 1593 (or use Find) to ensure there is exactly one declaration of `const supabase = window.supabase.createClient(...)`.
- Confirm no syntax errors are highlighted.

**Step 3: Verification complete**
- No code was changed, the workspace is now synced with production.
