# Knit Knot Sort — concept (condensed)

Source of truth: `Knit_Knot_Sort_Concept_Doc_4day.docx` (29 Oct 2026). Only Nawal changes design decisions; propose changes in `TRACKER.md` → Open questions.

## Pitch

A cozy low-poly colour-sort puzzle. A girl who owns a small yarn shop needs help sorting her yarn; every colour you sort becomes a stripe in a scarf she knits for you.

- **Genre:** layer colour sort (ball-sort / water-sort family). Nothing is literally tangled.
- **Platform:** CrazyGames Basic Launch (no ads yet). Unity URP, WebGL.
- **Orientation:** portrait first; desktop shows the same column with scenery at the sides.
- **Build budget:** 4 working days for launch scope.

## Rules

- The board has several **spools**. Each holds up to **4 yarn bands**, bottom to top.
- Some spools start **empty**: 2 early, 1–2 in the hardest levels.
- **Move:** tap a spool to lift its top band, tap another spool to drop it. Same-colour bands sitting together on top move together, as far as the target has room.
- **Legal drop:** onto an empty spool, or onto a band of the same colour with room left.
- **Complete spool:** 4 bands of one colour. It locks (can't be selected or dropped on) and knits one stripe onto the scarf.
- **Win:** every colour complete.
- **Stuck:** no legal moves. Show a friendly "Stuck!" prompt with Undo / Restart / Hint. No timer, no move limit, no fail screen.

Example start (R, B, Y colours; `_` empty), listed top to bottom:

```
 s1  s2  s3  s4  s5
 Y   R   B   _   _
 R   B   Y   _   _
 B   Y   R   _   _
 R   B   Y   _   _
```

## Player tools

| Tool | Launch behaviour |
| --- | --- |
| Undo | Free, unlimited |
| Restart | Free |
| Hint | Free in Basic Launch; highlights the next move from the solver |
| Extra spool | Free once per level; adds one empty spool |

## Levels

| Levels | Colours | Empty spools | Notes |
| --- | --- | --- | --- |
| 1–3 | 2–3 | 2 | Tutorial: hand pointer shows the first move, no text |
| 4–15 | 3–5 | 2 | Same-colour runs moving together |
| 16–30 | 5–7 | 2 | Larger boards; every 5th level is an easy breather |
| 31–40 | 7–8 | 1–2 | Hardest band |
| Endless | 6–8 | 1–2 | Generated at runtime |

Max board: 8 colours + 2 empty = 10 spools.

### Generator

1. **Deal:** shuffle 4 bands of each colour into the filled spools.
2. **Reject trivial deals:** any spool already complete, or too many bands already sitting on a matching colour.
3. **Solve:** depth-first search with memoised canonical board states (spool order sorted) and a move cap. Keep solvable deals only.
4. **Grade:** minimum moves plus dead ends explored gives a difficulty score.
5. **Order** within each band, insert breathers, export `levels.json` with each level's solution path (used for hints).

Do not generate by scrambling a solved board backwards: reverse moves are not always legal forward moves.

## Payoff

- A low-poly scarf hangs on a clothesline above the board, split into stripe sections. Each completed spool colours the next section, in completion order, so every scarf is unique.
- The last 5 finished scarves stay visible on the line.

## Story wrapper

- 2D dialogue overlay at the bottom of the screen: portrait (3 expressions: happy, surprised, proud) + text box, typewriter text, tap to advance, Skip always visible.
- She speaks after level 1, at levels 5, 15, 25 and 40, and briefly when a scarf is finished. Never during a puzzle.
- Lines live in `dialogue.json` (~15–20 lines).

## Art direction (final look, NOT for greybox)

Soft cel-shaded low-poly toy diorama: chunky rounded shapes; faceted environment, smooth yarn and spools; 2–3 tone toon ramp, no outlines; tinted (purple/olive) shadows; limited palette; perspective camera with narrow FOV (20–30°) at a high angle; spools in a shallow arc so every top band is visible. Up to 8 colour-blind-safe yarn colours, each with a small symbol.

## Layout (portrait)

1. Top bar: level number, pause.
2. Upper ~25%: clothesline with the scarf.
3. Middle ~50%: table with spools in an arc.
4. Bottom: Undo, Restart, Hint, Extra spool; dialogue overlay appears here between levels.

## Out of scope for launch

Yarn inventory, garment designer, hats/sweaters, sew-on patches, daily rewards, wild multicolour yarn (first update), locked spools, ads, Poki port.
