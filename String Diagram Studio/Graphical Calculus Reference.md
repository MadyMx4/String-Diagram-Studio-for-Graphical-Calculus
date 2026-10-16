# Graphical Calculus Reference

This reference documents the custom TikZ macros in `Lentner Diagrammatics/Fiber_functors.tex`, primarily the definitions in its “Braiding Diagrams” section. It describes what the source implements, not a claim that every macro has a unique categorical interpretation.

## Drawing conventions

- Diagrams are read from bottom to top, as stated in the source's string-diagram examples.
- Coordinates use TikZ's `(x, y)` convention. The macros accept coordinates and dimensions as arguments, so the source does not impose a grid or fixed cell size.
- Wires are thick and generally use rounded corners. The `\grau` color macro is `gray!60`; most primitives accept a color argument.
- Wire labels and object labels are placed with ordinary TikZ `\draw ... node {...}` commands, rather than by a dedicated label-placement API.
- Crossing examples are composed in drawing order. A later `\vLineO` is drawn over a white stroke, visually interrupting the earlier wire beneath it.

## Wire primitives

| Macro | Arguments | Behavior |
| --- | --- | --- |
| `\vhLine{x1}{y1}{x2}{y2}{color}` | start, end, color | Draws a rounded, multi-segment wire between the coordinates, with a horizontal bend in the middle. |
| `\vLine{x1}{y1}{x2}{y2}{color}` | start, end, color | Draws a rounded, multi-segment wire between the coordinates, with bends positioned along the route. Used for straight strands and slanted crossing strands. |
| `\vLineO{x1}{y1}{x2}{y2}{color}` | start, end, color | Draws a thick white version of the route, then draws the colored wire on top. This is the over-strand/knockout technique used to show crossing order. |
| `\joinPt{x1}{y1}{x2}{y2}{color}` | start, end, color | Alias for `\vLine`. |
| `\joinPtO{x1}{y1}{x2}{y2}{color}` | start, end, color | White knockout followed by `\vLine`; an overdraw variant of `\joinPt`. |

The line macros take coordinates rather than node identifiers. There is no built-in graph connectivity or grid snapping in these definitions.

## Hopf and related structural operations

| Macro | Arguments | Behavior in the source |
| --- | --- | --- |
| `\dMult{x}{y}{width}{height}{color}` | position, dimensions, color | Draws a three-legged junction: two legs at the lower/starting level and a central leg at the level determined by signed `height`. The source uses it for multiplication diagrams; changing the sign flips the junction vertically. |
| `\dMultO{x}{y}{width}{height}{color}` | position, dimensions, color | Knockout/white-underlay variant of `\dMult`, followed by the colored junction. |
| `\dUnit{x}{y}{color}` | position, color | Draws a short wire ending in an open circle below the given point. |
| `\dCounit{x}{y}{color}` | position, color | Draws a short wire ending in an open circle above the given point. |
| `\dAntipode{x}{y}{color}` | position, color | Draws a vertical wire with an open circle centered on it. |
| `\dSkewantipode{x}{y}{color}` | position, color | Same wire-and-circle shape as `\dAntipode`, but the circle is gray-filled. |

The source groups these under “Structural morphisms.” `\dMult` is the only dedicated multiplication/comultiplication-shaped junction macro; its signed height can reverse its orientation, but the file does not define a separately named `\dComult` macro.

## Other labeled morphisms

| Macro | Arguments | Behavior |
| --- | --- | --- |
| `\dAction{x}{y}{width}{height}{color1}{color2}` | position, dimensions, two colors | Draws a bent first strand and a vertical second strand in separate colors, meeting at the same horizontal level. |
| `\dPairing{x}{y}{width}{height}{color}{label}` | position, dimensions, color, label | Draws a cap-like connection between two endpoints and places the label at its center. |
| `\dCopairing{x}{y}{width}{height}{color}{label}` | position, dimensions, color, label | Draws the corresponding cup-like connection and places the label at its center. |

The pairing macros are the cap/cup drawings for evaluation and coevaluation: `\dPairing` draws the evaluation cap, and `\dCopairing` draws the coevaluation cup. Their `label` argument can be used for `\ev` or `\coev`; the separate `\ev` and `\coev` commands elsewhere in the file are text operators, not drawing macros. Strand order may be adjusted for the left- or right-dual convention being depicted.

## Not provided as dedicated drawing macros

- **Comultiplication:** no separately named `\dComult` macro; the signed `\dMult` geometry is the only related junction primitive.
- **Evaluation and coevaluation:** rendered by the generic `\dPairing` (evaluation cap) and `\dCopairing` (coevaluation cup) macros; there are no separately named `\dEval`/`\dCoeval` graphics macros. `\ev` and `\coev` are text commands.
- **Crossings:** no crossing/braiding node macro. Crossings are made from wire segments, with `\vLineO` or `\joinPtO` providing the white knockout that indicates which strand passes over.
- **Grid, ports, and line labels:** the macros use free TikZ coordinates. They do not implement grid cells, edge ports, snapping, or interactive labels.

For an editor palette, these definitions support wire segments, over-strand rendering, multiplication-style junctions, unit/counit, antipode/skew-antipode, action, evaluation/co-evaluation caps and cups, and crossings composed from wire segments. A separately named comultiplication tool and a dedicated crossing macro would be editor-level operations, not existing macros in this file.