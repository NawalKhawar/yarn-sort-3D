# Knit Knot Sort — tracker

Claude updates this file after every task. Nawal can edit it too.
Status key: `[ ]` to do · `[~]` in progress · `[x]` done · `[-]` cut

## Milestone 1 — Greybox (primitives only)

**Done when:** levels 1–40 are generated and playable with cylinders in the final camera and portrait layout, every top band is visible and easy to tap on a phone-sized Game view, and undo/restart work.

### 1A. Project setup
- [ ] Folder structure under `Assets/_Project/` and assembly definitions (`KKS.Logic` with no engine references, `KKS.View`, `KKS.Editor`, `KKS.Tests.EditMode`)
- [ ] Unity Test Framework set up; one passing placeholder test
- [ ] `Greybox` scene with root organiser objects; Game view set to a 1080×1920 portrait resolution

### 1B. Logic layer (plain C#, unit-tested)
- [ ] `BoardState`: spools of up to 4 bands, colour ids, empty spools, completed/locked flags, clone and canonical key
- [ ] Move rules: legal-move check, same-colour run moves as far as room allows, spool completion
- [ ] Undo stack and restart to initial state
- [ ] Win and stuck detection
- [ ] Tests: move legality, run moves, completion, win, stuck, undo

### 1C. Generator and solver
- [ ] Solver: DFS with memoised canonical states, move cap, returns minimum solution path and dead-end count
- [ ] Generator: deal, reject trivial, solve, grade (per CONCEPT.md bands)
- [ ] Editor window (`KKS > Level Generator`) with band settings in the Inspector, seed field, Generate button
- [ ] Export `levels.json` (40 levels + solution paths); breathers every 5th level from 16
- [ ] Test: every exported level replays to a win with its stored solution

### 1D. Greybox view
- [ ] `LayoutConfig` ScriptableObject (spool spacing, arc radius/angle, band height, spool radius, camera angle/FOV/distance)
- [ ] `ColourPalette` ScriptableObject (8 placeholder colours)
- [ ] Spool and yarn-band prefabs from cylinders, with capsule colliders for tapping
- [ ] Board view builds spools from a level in an arc; refreshes from logic state after every move
- [ ] Camera framing: perspective, narrow FOV, high angle; auto-fits 10 spools in portrait; centred column on wide aspect ratios

### 1E. Input and flow
- [ ] Tap/click to select (selected spool lifts), tap to drop; illegal drop gives a small shake
- [ ] Load level from `levels.json`; level complete → next level; progress saved locally
- [ ] Greybox UI buttons: Undo, Restart, Hint (logs/highlights next move), Extra spool, level number
- [ ] Stuck prompt (placeholder panel)
- [ ] Greybox scarf: a row of 8 quads that colour in, in completion order

### 1F. Regression safety net
- [ ] All logic tests run in one command (Test Runner → EditMode → Run All) in under ~30 s
- [ ] Level replay test covers all 40 levels in `levels.json`
- [ ] Save/load round-trip test
- [ ] Manual greybox checklist written below (Playtest notes) for Nawal to repeat after big changes

### 1G. Greybox playtest (Nawal)
- [ ] Play levels 1–40 in the phone-sized Game view; note difficulty spikes and tap problems below
- [ ] Decide final camera angle and spool arrangement

## Milestone 2 — Look and feel (Day 2)
- [ ] Lock palette; Shader Graph toon material; style test scene vs reference
- [ ] Model kit (spool, yarn band, table, scarf, tree, rock, fence, porch); swap into prefabs
- [ ] DOTween motion (lift, wind, bob, squash) and completion particles

## Milestone 3 — Payoff, story and flow (Day 3)
- [ ] Scarf stripe knitting + clothesline (last 5 scarves)
- [ ] 2D dialogue overlay, 3 portraits, `dialogue.json`
- [ ] Final UI kit, Stuck panel, tutorial levels 1–3 hand pointer
- [ ] Endless mode

## Milestone 4 — Platform and release (Day 4)
- [ ] CrazyGames SDK (gameplayStart/Stop) and Data module saves
- [ ] WebGL build optimisation, size check (< 20 MB)
- [ ] Device testing: phone, tablet, low-end laptop, Safari
- [ ] Audio pass; store covers and gameplay video; upload for Basic Launch

## Cut list (in order, if behind)
1. Endless mode
2. Clothesline of past scarves
3. Colour symbols on yarn
4. Mid-game dialogue beats
5. Levels 31–40

Never cut: scarf payoff, solver-checked levels, portrait layout, saves, toon look on spools and yarn.

## Decisions log
| Date | Decision | Why |
| --- | --- | --- |
| 2026-10-29 | Generate by random deal + solver check, not reverse scramble | Reverse moves aren't always legal forward moves |
| 2026-10-29 | Dialogue is 2D overlay, no 3D character | Cheaper; plays to 2D art strength |

## Open questions
- Final title ("Knit Knot Sort" implies untangling)
- Shop owner's name, personality, shop name
- Scene setting: shop porch or cottage garden
- Hint: full next move, or highlight one spool only?

## Backlog (later, not this milestone)
- Wild multicolour yarn, yarn inventory, garment designer, hats/sweaters, patches, daily rewards

## Playtest notes
_(Nawal adds notes here)_

## Bug log
| Date | Bug | Regression test name | Fixed? |
| --- | --- | --- | --- |
| | | | |

## Session log
| Date | What was done | New Inspector properties / assets | Tests (count / result) | Notes |
| --- | --- | --- | --- | --- |
| | | | |
