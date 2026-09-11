---
name: create-workshop-mini-games
description: >-
  Create, split, restyle, or refactor student-facing simulations, games, and puzzles
  for the Rasta Internet Workshop. Use for new interactive learning games and for
  converting large single-file or existing multi-file games to the repository's Vite,
  multi-page, RTL, visual, and discovery-learning conventions.
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
Use square UI edges. Select one of the desaturated two-to-four-color sets in
`docs/mini-games.md`; do not combine palettes. Use color purposefully and pair it with
another visual cue.

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
repository tests and build. Report the paths created or changed and the observed
verification results.
