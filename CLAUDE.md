# Knit Knot Sort — instructions for Claude

You are helping build **Knit Knot Sort** (working title), a cozy low-poly yarn colour-sort puzzle for CrazyGames, in Unity (URP, WebGL). Nawal is the creative lead and does review and QA; you write the code and drive the Unity Editor through the Unity MCP.

Read these before doing anything else. They are imported here, so they are always in context:

@Docs/CONCEPT.md
@Docs/TRACKER.md

## Current phase

**Greybox.** Unity primitives only (cylinders, cubes, planes). No final models, shaders, art or audio yet. The goal is a fully playable game with placeholder shapes, in the final camera angle and portrait layout. See Milestone 1 in `Docs/TRACKER.md`.

## How to work

1. **Start of every session:** read `Docs/TRACKER.md`, say in 2–3 lines where the project stands and which task is next, then wait for Nawal to confirm before starting.
2. **One task at a time.** Pick the next unchecked task in the current milestone. Before writing code for anything non-trivial, give a short plan (files you'll create or change, and how Nawal will test it in the Editor).
3. **After each task:**
   - Check the Unity console through the MCP for compile errors and warnings. Fix them before saying a task is done.
   - Run the EditMode tests if logic changed, and report the result.
   - Update `Docs/TRACKER.md`: tick the task, add a line to the Session log, and record any decision in the Decisions log.
   - Tell Nawal exactly how to test it: which scene to open, what to press, what she should see.
4. **Never mark a task done** if it doesn't compile, a test fails, or Nawal hasn't been told how to check it.
5. **Ask before:** deleting files or GameObjects, changing project or player settings, installing packages, or changing anything in `Docs/CONCEPT.md` (design changes are Nawal's call). If a design question comes up, add it to Open questions in the tracker and ask.
6. **Don't start work outside the current milestone** unless Nawal asks. If you notice something for later, add it to the Backlog section of the tracker.

## Code architecture

- **Logic layer** (`Assets/_Project/Scripts/Logic/`, assembly `KKS.Logic`): plain C# — board state, moves, undo, win/stuck checks, generator, solver. The asmdef has `noEngineReferences: true`, so **no UnityEngine types here**. Everything here is unit-testable.
- **View layer** (`Assets/_Project/Scripts/View/`, assembly `KKS.View`): MonoBehaviours that mirror the logic state — spool and yarn-band prefabs, input, camera framing, UI. The view never holds game rules; it asks the logic layer.
- **Editor tools** (`Assets/_Project/Scripts/Editor/`, assembly `KKS.Editor`): level generator window that writes `levels.json`.
- **Tests** (`Assets/_Project/Tests/EditMode/`, assembly `KKS.Tests.EditMode`): Unity Test Framework, EditMode only.
- **Data:** `Assets/_Project/Resources/Levels/levels.json`; tuning values in ScriptableObjects under `Assets/_Project/Config/`.
- Namespace everything under `KKS.*`. One class per file; file name matches class name.

## Testing and regression tests (important)

A regression test checks that something which already works keeps working after later changes. The full EditMode test suite is the safety net: run it after every task.

- **Every logic feature gets tests when it's written** (moves, undo, win/stuck, generator, solver, save/load).
- **Every bug fix gets a regression test first:** write a test that reproduces the bug and fails, fix the bug, confirm the test now passes. Name it after the bug, e.g. `Undo_AfterSpoolCompletes_RestoresUnlockedSpool`, and add a one-line comment saying what broke.
- **Level regression test:** every level in `levels.json` must replay to a win with its stored solution. Re-run it whenever the generator or rules change.
- **Save regression test:** saved progress from the previous format must still load (or migrate) after save-format changes.
- **Run the whole suite**, not just new tests, before marking any task done. If an old test fails, stop and report it: don't change an old test to make it pass unless Nawal agrees the old behaviour was wrong.
- Keep tests fast (whole suite under ~30 seconds) and independent of scenes.
- Record the test count and result in the Session log.
- View-layer behaviour (tapping, camera framing) is checked by Nawal by hand; give her a short manual checklist in "How to test".

## Inspector rules (important)

Every value a designer might tune must be editable in the Inspector, never a magic number in code.

- Use `[SerializeField] private` fields, not public fields.
- Group with `[Header("...")]`; explain each field with `[Tooltip("...")]` in plain English, including units (seconds, metres, degrees).
- Constrain with `[Range(min, max)]` or `[Min(0)]` wherever a sensible range exists.
- Game-wide tuning (layout spacing, animation durations, colours, camera angle, band height, etc.) lives in ScriptableObject assets (`GameConfig`, `LayoutConfig`, `ColourPalette`) with `[CreateAssetMenu]`, so Nawal can tweak values without touching code or scenes.
- When you add a new tunable property: create or update the asset, give it a sensible default, and **tell Nawal its name, where it is, and what it does**. Also note it in the Session log.
- Use `OnValidate` to refresh greybox visuals in the Editor when a layout value changes, where practical.

## Unity MCP usage

- Prefer creating and editing scenes, GameObjects, prefabs and components through the MCP rather than asking Nawal to do it by hand. When something must be done by hand, give numbered click-by-click steps.
- After creating or editing scripts, wait for recompilation, then read the console before continuing.
- Save scenes and prefabs after changes.
- Keep the scene tidy: root objects named `--- Systems ---`, `--- Board ---`, `--- Camera ---`, `--- UI ---` as parents.
- Don't touch `Library/`, `Temp/`, `Logs/`, `UserSettings/` or `ProjectSettings/` unless asked.

## Platform constraints (CrazyGames, WebGL)

- Portrait first (9:16). Must also look right in desktop iframes 800×450 up to 1920×1080: the portrait column is centred.
- Land in gameplay within one click: level 1 starts immediately.
- Mouse and touch both work; no keyboard required; never bind Escape.
- Keep the initial download under 20 MB; avoid heavy packages.
- Must run smoothly on a 4 GB Chromebook: no post-processing, few real-time lights.
- CrazyGames SDK and ads come in a later milestone, not now.

## Style of communication

Nawal's background is game design, art and UI/UX with a CS degree. Explain Unity-specific steps plainly; keep summaries short; always end a task with "How to test" steps.
