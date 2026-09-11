---
name: create-workshop-mini-games
description: >-
  Create, split, restyle, or refactor student-facing simulations, games, and puzzles
  for the Rasta Internet Workshop. Use for new interactive learning games and for
  converting large single-file or existing multi-file games to the repository's Vite,
  multi-page, Persian-language, RTL, visual, and discovery-learning conventions.
---

# Create Workshop Mini-games

Create a focused interactive system that helps students discover a concept by
exploring a simulation, solving a game or puzzle, or doing both. A game may have
several variables, explicit constraints, scoring, a mechanical win state, and a high
level of difficulty. Completing it does not replace mentor acceptance of the workshop
step.

Before changing a game, read:

- [`docs/mini-games.md`](../../../docs/mini-games.md) for the complete product, visual,
  architecture, migration, and verification contract.
- `## قراردادن بازی‌ها` in
  [`docs/development.md`](../../../docs/development.md) for current publishing details.
- [`docs/workshop-rendering-brief.md`](../../../docs/workshop-rendering-brief.md) and
  [`docs/step-template.md`](../../../docs/step-template.md) when the game is tied to a
  workshop step or its learning progression.

## Define the learning interaction

Identify the concept to visualize; whether the experience is an open simulation, a
goal-oriented puzzle, or a hybrid; the variables students may change; and the intended
insight that must remain unstated. For a puzzle, define its operational goal,
constraints, scoring or success feedback, and retry loop. Inspect the relevant student
and mentor step content when available, but never move mentor-only content, hints,
acceptance criteria, or the final explanation into the game.

Favor direct, reversible manipulation with prompt visual feedback. Preserve the
student's current work across incomplete or failed attempts. Simulations should show
consequences and trade-offs; puzzles may report scores, failure, or a mechanical win
without revealing the intended strategy or claiming conceptual understanding. Leave
step acceptance to the mentor.

Allow as many meaningful variables and controls as the challenge requires. Keep the
system visually dominant and remove only accidental complexity. A puzzle may be hard
to beat, but its difficulty should come from the concept and interacting constraints,
not unclear rules, slow feedback, hidden state, excessive typing, or awkward controls.

## Choose the implementation shape

Place the project at `games/<project-name>/` and use Vite even for vanilla
HTML/CSS/JavaScript. Keep dependencies local and committed through `package-lock.json`;
do not require a CDN at workshop runtime. Use p5.js only when its canvas and animation
model materially simplifies the simulation.

For one game, build `index.html` to `dist/index.html`.

For multiple student-facing versions:

- Create one root `index.html` hub plus one `<version>/index.html` per version.
- Never keep all versions as modes inside one large HTML page.
- Share logic and styles through modules, not through shared navigation.
- Link from the hub with explicit relative paths such as
  `./<version>/index.html`.
- Do not put home, sibling, next/previous, or breadcrumb links on version pages.
- Register the root and every nested HTML page in Vite's
  `build.rollupOptions.input` and preserve each path under `dist/`.

When the input is a large HTML file or an existing game, first inventory its versions
and behavior. Then separate its pages, extract focused CSS and JavaScript modules, add
Vite and local dependencies, and apply the workshop interaction and visual system.
Preserve useful simulation behavior; do not introduce a frontend framework without a
concrete need.

## Apply the workshop visual system

Use a clear Persian-first RTL interface with a dominant play or simulation area,
concise text, minimal typing, and no unnecessary controls. Do not remove meaningful
planning tools, variables, or status displays solely to make a complex game sparse.
Use four or five distinct, lively chromatic colors from the event-derived palette in
`docs/mini-games.md`, in addition to neutral canvas and ink colors. Assign each hue a
stable functional meaning—such as an actor, state, path, or category—and do not spend
colors on decoration. Use clear, restrained color coding where it makes the system
easier to scan, and pair color with shape, position, pattern, line style, motion, a
symbol, or a short Persian label.

For selection, focus, hover, and active-state highlighting, default to a soft
translucent halo or tint plus one modest outline. Keep paths and borders light enough
that they do not overpower labels or objects. Do not stack thick outlines, oversized
frames, or rigid dashed borders merely to make a state noticeable. Give rectangular
interactive objects and section containers a small corner radius by default; avoid
pill shapes unless the represented object is naturally pill-shaped. Stronger emphasis
is appropriate only when the state has exceptional semantic urgency.

Use clearly contrasting background colors for distinct sections such as the play
area, controls, queues, endpoints, or results. Adjacent regions must differ visibly in
hue or lightness rather than relying only on shadows or subtle borders. Prefer
readable tints of the functional palette for large surfaces, retain sufficient
foreground contrast, and verify the separation at iframe size and on a low-contrast
display or projector.

Do not render a title, nameplate, or static explanatory description at the top of a
game page. Make the interaction self-explanatory through its initial state,
affordances, labels, visible constraints, motion, and feedback. Use only short
contextual labels, changing status text, or just-in-time hints when needed. Keep an
HTML `<title>` for document metadata and the workshop's game index; it must not become
a visible in-game heading. A multi-version root hub may identify its collection
because it is not itself a game screen.

Write every student-visible string in Persian, including controls, feedback, scores,
errors, tooltips, document titles, alternative text, canvas fallbacks, and
accessibility names. Use English only for specific networking terminology that has no
common Persian equivalent, and match the terminology in the relevant step. In
particular, use `بسته`, `پیام`, and `مسیریابی` instead of `packet`, `message`, and
`routing`; a specialist term such as `switch` may remain when appropriate. English is
fine in source code and developer-only text. Keep conventional Latin scientific and
technical symbols such as `V`, `s`, `ms`, `Hz`, `bps`, and formula variables; these
are not English copy and must not be translated. Add a Persian label when useful and
isolate Latin UI fragments as LTR.

Use Vazirmatn for every Persian UI string, including DOM, SVG, and Canvas text. Bundle
the font or import a local package through Vite; do not fetch it from a runtime CDN.
Monospace may still be used for code, bit strings, and similar technical data. Ensure
the font has loaded before measuring or drawing Canvas or p5.js text.

Animate cause and effect smoothly without delaying interaction. Respect
`prefers-reduced-motion`. Make the game responsive in the workshop iframe, fullscreen,
and a standalone page, with touch, mouse, and reasonable keyboard support.

## Integrate and verify

Use `http://games/<project>/<version>` in Google Docs step content, or the equivalent
iframe ending in `/index.html` in committed Markdown. A multi-version project's root
hub belongs only on the workshop home page; steps must target a version directly.

Build the game and confirm `dist/index.html` plus every nested
`dist/<version>/index.html`. Verify relative imports and assets from the final nested
path, check that version pages expose no cross-game navigation, then run the
repository tests and build. Check that no game page begins with a visible title or
static description, that Persian text renders in Vazirmatn without network access,
that four or five functional colors remain distinct and consistent, and search the
rendered UI and source strings for accidental English student-facing copy, excluding
permitted specialist terms and conventional symbols. Confirm that adjacent section
backgrounds remain clearly distinguishable and their contents readable. Check that
highlight states use soft halos or tints, a single modest outline, and slightly rounded
corners without sacrificing visibility. Report the paths created or changed and the
observed verification results.
