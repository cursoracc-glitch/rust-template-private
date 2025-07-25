---
trigger: always_on
description: 
globs: 
---
# Rust Plugin Development Guide (for uMod)

This rule describes the **full development workflow** for creating high-quality, production-ready uMod plugins for Rust using Cursor AI in autonomous mode.

## Step 1: Gather Complete Requirements (TЗ)

- Always begin with full collection of technical requirements from the user.
- Ask clarifying questions to cover edge cases, command structure, configuration behavior, and player interaction.
- Log assumptions and validate intent before coding anything.
- During this phase, **run in strict order**:
  1. `search_codebase(...)` — search for related plugins, utility classes, and in-code precedents.
  2. `search_web(...)` — check uMod forums, GitHub, and plugin directories to locate reusable solutions or design patterns.
- Back-prompting is allowed and encouraged — use natural language queries to discover plugin matches, not only names.

## Step 2: Scaffold Plugin Folder and Metadata

- Create a subfolder under `plugins/` using the plugin's internal name.
- **Always** generate a `README.md` file as the first file:
  - Include full, approved technical specification (TЗ)
  - Document all of the following:
    - Purpose and features
    - Permissions
    - Commands
    - Chat or console interaction
    - Configuration layout
    - Any assumptions or user-specific notes
- The `README.md` must be shown to the user and explicitly confirmed **before any code is written**.
- After confirmation, **repeat both**:
  1. `search_codebase(...)` — to verify internal consistency
  2. `search_web(...)` — to confirm latest community approaches

## Step 3: Cross-Reference Existing Solutions

- Before implementation, check if similar plugins already exist.
- Use:
  - `grep_codebase("PluginName")` or by functionality keywords
  - `search_web("oxide PluginName")` or `search_web("uMod plugin XYZ")`
- Evaluate design decisions and borrow robust patterns when appropriate.
- Never reinvent what can be safely reused or improved.

## Step 4: Plan, Implement, and Validate

- Break the plugin into submodules and task steps.
- Create a plan and get approval before implementing core logic.
- Follow best practices:
  - Use `permission.RegisterPermission(...)`
  - Use `lang.RegisterMessages(...)` with localization keys
  - Load all config settings via `LoadConfig()` and `LoadDefaultConfig()`
  - Use `BasePlayer` for Rust-specific access; use `IPlayer` and `[Command]` for abstracted logic (CovalencePlugin only)
  - Use `[ChatCommand]` and `[ConsoleCommand]` when targeting BasePlayer (`RustPlugin`)

## Step 5: Compile and Fix Every Error

- Run `dotnet build` **after every significant change**.
- You MUST fix all compiler errors and warnings — there is no "partial implementation" allowed.
- If errors occur:
  - Re-run `search_codebase(...)` or `search_web(...)`
  - Investigate error cause
  - Patch and rebuild
- Repeat this cycle until zero errors remain.

## Step 6: Final Testing and Runtime Diagnostics

- Once plugin compiles cleanly, deployable version must be confirmed:
  - Manually test each command
  - Confirm expected behavior on edge cases
  - Simulate plugin load/unload scenarios

- If user reports runtime exceptions **after deployment**:
  - Add detailed log messages (e.g. `Puts("Reached teleport logic")`) to suspected lines
  - Ask user to install plugin on a live server and return logs
  - Use logs to iteratively improve stability
  - Only valid at this stage if compilation is fully successful

## Summary

- This process is **non-negotiable**.
- Your job is to deliver fully working, documented, and testable plugin logic — not fragments or ideas.
- You may not skip steps.
- You must drive development through planning, validation, fixing, and confirmation.

> Failure to complete any of the steps above constitutes an incomplete plugin.
> You are the agent responsible for full delivery — from request to result.