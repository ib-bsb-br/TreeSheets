# TreeSheets Zoom/Scale "Unclucking" Discovery Brief

## Goal
Translate the forum comment below into concrete, testable engineering signals so follow-up fixes can target the real pain points instead of guessing.

> "Could someone dedicate some time towards, uh, 'unclucking' the zoom/scale system given its not a physical zoom but the mess that it is?"

## 1) What the comment likely means (semantic decomposition)

The user appears to be pointing at **model mismatch** between what people call "zoom" and what TreeSheets currently does in several different subsystems:

1. **Hierarchical view depth navigation** (zoom in/out into nested cells), implemented by modifying `drawpath` depth.
2. **Presentation fit scaling** (F12/Scaled mode), implemented via `dc.SetUserScale(currentviewscale, currentviewscale)` only in certain draw conditions.
3. **Text size changes** (Shift+wheel) that alter relative font sizing (`RelSize`) rather than camera magnification.
4. **Image DPI/display scaling** with separate scale semantics (`display_scale`) for bitmaps.
5. **Print scale** with its own separate code path.

Because these all involve "size" or are surfaced with zoom-like language, users experience one umbrella concept but the codebase implements multiple non-equivalent behaviors.

## 2) Evidence in current code (ground truth)

### A. "Zoom" actions are depth/path navigation, not camera magnification
- `A_ZOOMIN` / `A_ZOOMOUT` dispatch into `Wheel(... ctrl=true ...)`.
- `Wheel()` with `ctrl` loops `Zoom(dir)`.
- `Zoom()` updates the hierarchical `drawpath` (via `ZoomSetDrawPath`) and refreshes layout; it does **not** apply a global geometric camera multiplier.

Implication: user expectation of smooth scale changes conflicts with depth-jump behavior.

### B. Physical rendering scale exists, but only in scaled presentation mode
- In `Draw()`, `currentviewscale` is calculated from viewport/layout ratio.
- Actual `SetUserScale(currentviewscale, currentviewscale)` is applied only when `scaledviewingmode && currentviewscale > 1`.
- Outside scaled mode, `currentviewscale` is reset to `1` and normal scrolling/virtual size is used.

Implication: "physical zoom" exists only conditionally, which is easy to perceive as inconsistent.

### C. Mouse wheel multiplexes different semantic operations behind modifiers
- `OnMouseWheel()` routes to:
  - Alt => column width resize,
  - Shift => text size change,
  - Ctrl => hierarchical zoom (drawpath depth),
  - none => scrolling.
- Optional inversion via `zoomscroll` preference further shifts mental model.

Implication: wheel behavior is powerful but cognitively overloaded.

### D. Font sizing also includes path-dependent bias
- `TextSize()` includes `pathscalebias` computed from `currentdrawroot->MinRelsize()`.
- This means apparent size may change from navigation context even without explicit text resize.

Implication: users may interpret path-relative text normalization as random zoom/scale drift.

### E. Additional scale channels increase ambiguity
- `Image::display_scale` is independent from document zoom logic.
- `printscale` is independent from both drawpath zoom and scaled-view fit logic.

Implication: "scale" appears in UI/features with different units and invariants.

## 3) Most plausible user pain statements hidden inside the comment

The single sentence likely compresses several frustrations:

1. **Naming frustration**: "zoom" label promises camera zoom, but behavior is mostly hierarchical focus change.
2. **Predictability frustration**: similar gestures lead to different "size" outcomes depending on mode/modifier/path depth.
3. **Continuity frustration**: moving between cells/depths changes perceived text size unexpectedly (`pathscalebias`).
4. **Mode-opacity frustration**: scaled presentation mode (F12) is distinct but not strongly represented as a different rendering regime.
5. **Interaction debt frustration**: wheel + modifier + optional inversion creates accidental operations.

## 4) Precision questions to ask users before fixing anything

To avoid solving the wrong problem, collect these in the forum thread or issue:

1. When you say "physical zoom", do you mean a continuous 25/50/100/125/150% viewport magnification that keeps document semantics unchanged?
2. Is your main pain with Ctrl+wheel zoom specifically, toolbar/menu zoom actions, or F12 scaled mode?
3. Which inconsistency hurts most:
   - zoom changing nesting depth,
   - zoom not preserving focal point,
   - text size changing unexpectedly,
   - wheel modifiers easy to trigger accidentally?
4. Should hierarchical navigation be renamed (e.g., "Enter Cell" / "Exit Cell") instead of "Zoom In/Out"?
5. Should text resize and camera zoom be strictly separated in UI and shortcuts?
6. Are you using touchpad pinch or only mouse wheel?
7. OS/build and DPI setup (Windows/Linux/macOS, scaling %, monitor DPI) to isolate rendering-vs-logic issues.

## 5) Reproduction matrix for maintainers (minimal but high signal)

Record behavior and expected behavior for each row:

1. Ctrl+wheel in normal mode.
2. Toolbar Zoom In/Out.
3. F12 toggle at large and small documents.
4. Shift+wheel text size on nested grids.
5. Alt+wheel column width on nested grids.
6. Transition into/out of tiny cells (`ZoomTiny` paths).
7. Deletion or structural edits near drawroot (watch `drawpath` adjustments).
8. Same operations with `zoomscroll` preference enabled.

Capture:
- selection location,
- drawpath depth,
- pathscalebias,
- currentviewscale,
- whether scaledviewingmode is active,
- resulting visual and status message.

## 6) Suggested issue framing (for accurate implementation work)

Use this language in a GitHub issue:

**Title:** Clarify and separate TreeSheets "zoom" semantics (hierarchy navigation vs viewport scale vs text scale)

**Problem statement:**
Current "zoom" terminology and controls combine multiple distinct mechanisms (hierarchical drawpath navigation, presentation fit scaling, text relative size, and image/print scaling), causing user confusion and inconsistent expectations.

**Acceptance criteria for discovery phase:**
1. Document current behavior matrix and terminology map.
2. Decide canonical names for each scale concept.
3. Define whether Ctrl+wheel should remain hierarchy navigation or become true viewport zoom.
4. Ensure UI labels/tooltips/status messages reflect chosen model.
5. Propose migration path preserving existing workflows (possibly with settings toggle).

## 7) Low-risk tactical improvements before any deep refactor

1. **Terminology-only patch**:
   - rename menu/tooltips from "Zoom In/Out" to "Drill In/Out" or "Enter/Exit Nested Cell" if behavior stays path-based.
2. **Status transparency**:
   - include mode/depth in status text (e.g., "Depth 3, Fit Scale 1.00x, Text RelSize +1").
3. **Command separation in UI**:
   - expose "Viewport Scale" separately from hierarchy navigation if physical zoom is introduced.
4. **Shortcut clarity**:
   - make wheel behavior discoverable in status/help; show active modifier mapping.

These changes can reduce frustration even before algorithmic changes.

## 8) Why this interpretation is high confidence

The code directly reflects non-unified concepts under overlapping names:
- hierarchical "zoom" via `drawpath` mutations,
- conditional view scaling via `scaledviewingmode/currentviewscale`,
- text scaling via `RelSize/pathscalebias`,
- independent image and print scales.

So the forum phrasing "not a physical zoom" is an accurate user-level diagnosis of conceptual coupling in the current UX model.
