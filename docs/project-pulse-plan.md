# Mona's Project Pulse Dashboard Implementation Plan

## Summary

This plan implements Mona's Project Pulse dashboard as a lightweight static web app in the repository's `app/` folder, coordinated by the repository's custom agent system under `.github/agents/`. The work follows the repository's orchestration model: the Orchestrator coordinates the overall build, the Planner scopes requirements and risks, the Designer shapes the UI/UX, and the Coder implements the code. The target result is a polished dashboard landing page that presents project health, status, priorities, and progress at a glance without requiring a framework or backend.

The dashboard should feel intentionally product-like and deterministic: a clear header, project cards, status badges, concise priority markers, and responsive spacing. The implementation is intentionally static and low-risk so it can run in a Codespace or local browser without extra infrastructure. The design is anchored by the repository's Project Pulse expectations and by the custom agent definitions in `.github/agents/designer.agent.md`, `.github/agents/coder.agent.md`, and `.github/agents/orchestrator.agent.md`.

## Repository-based context and agent alignment

- `.github/agents/orchestrator.agent.md` defines the workflow: break work into phases, keep file scopes explicit, avoid overlapping edits, and verify integration before finishing.
- `.github/agents/designer.agent.md` requires a polished dashboard with clear hierarchy, responsive behavior, and visible project-card styling hooks such as `.dashboard` and `.project-card`.
- `.github/agents/coder.agent.md` requires predictable file structure, explicit validation, and support for runnable app tasks including `.vscode/launch.json`.
- The repo is small and intentionally focused, so the plan keeps the app self-contained within the `app/` folder and uses a simple launch configuration to preview the dashboard in-browser.

## Goal and deliverable

The deliverable is a single-page dashboard preview with:

- a dashboard shell and page title
- a visible project overview area
- project cards populated from structured JSON data
- logical priority and health status treatment
- responsive, readable styling
- a method to run the app locally from the `app/` directory via VS Code launch configuration

## File assignments

### 1) `app/index.html`

Owner: Coder with design review from Designer.

Responsibilities:

- Create the semantic HTML structure for the Project Pulse dashboard.
- Include a page header, dashboard summary section, and project card container.
- Add the expected hooks for CSS classes such as `.dashboard`, `.project-card`, `.status-badge`, `.priority`, and supporting layout containers.
- Reference the stylesheet and data source using relative paths that work when the app is served from the `app/` directory.
- Keep the markup semantic and stable enough for CSS to style without depending on brittle selectors.
- If needed, include a minimal script to fetch and render `project-data.json` into the DOM, or render fallback content if the JSON file is unavailable.

Implementation notes:

- The HTML should be static-first and resilient: even if JS fails, the page should still present at least a basic dashboard shell.
- Use descriptive section labels and accessible text for status, priority, and due-date content.
- Keep the root structure simple enough for the Designer to polish with CSS without additional HTML restructuring.

### 2) `app/styles.css`

Owner: Designer with Coder review for implementation correctness.

Responsibilities:

- Define the visual system for the dashboard: spacing, typography, color tokens, card layout, badges, and responsive behavior.
- Implement a polished Foundation for project cards with clear status differentiation and strong hierarchy.
- Ensure the design reads as a product dashboard in the first viewport, not as a generic landing page.
- Include responsive breakpoints for wide desktop, tablet, and narrow device widths.
- Add accessible contrast and legible body text.

Implementation notes:

- Use deterministic hooks like `.dashboard`, `.project-card`, `.status-badge`, `.project-summary`, and other explicit selectors.
- Use rounded corners, shadow, mild borders, clear spacing, and color-coded statuses to express priority and health.
- Avoid decorative excess; prioritize clarity and scanability.

### 3) `app/project-data.json`

Owner: Coder.

Responsibilities:

- Define the data model for a list of projects and associated fields such as name, owner, status, priority, progress, due date, or health metrics.
- Keep the schema consistent with the HTML rendering logic.
- Ensure the JSON is valid and easy for a frontend script to consume.
- Include realistic sample values and at least one edge-case scenario, such as a project with a neutral or delayed status.

Implementation notes:

- The schema should be explicit and stable enough that the Designer and Coder can agree on visual interpretation.
- Include sample statuses like `on-track`, `at-risk`, `blocked`, or equivalent labels consistent with the project design.
- Keep values simple and deterministic for testing.

### 4) `.vscode/launch.json`

Owner: Coder.

Responsibilities:

- Add a valid VS Code launch configuration to run the app from the repository.
- Set the launch configuration `cwd` to `${workspaceFolder}/app` so the app is launched from the dashboard folder.
- Open `index.html` so learners see the dashboard rather than a directory listing.
- Prefer deterministic paths and a local preview command that serves static files safely.

Implementation notes:

- The configuration should support opening the app in a browser with the minimum required local server configuration.
- It must be strict JSON and rely on a static preview strategy that does not require a framework or build step.
- A lightweight local HTTP server such as Python's `http.server` is a safe option if the environment supports it.

## Designer responsibilities

The Designer owns the product feel and the information hierarchy for the dashboard.

Detailed responsibilities:

- Create the visual direction for the dashboard so it reads as a Project Pulse product, not a raw HTML mock.
- Define the card layout and overall structure in a way that makes projects easy to scan.
- Establish visual conventions for:
  - status badges
  - priority emphasis
  - progress indicators or progress-value text
  - spacing and typography
  - primary/secondary actions or summary information
- Ensure the first impression communicates project health, urgency, and workload without requiring the user to read long blocks of text.
- Review the produced HTML for semantic quality and ensure the code structure supports the final design decisions.
- Validate accessibility concerns such as contrast, responsive legibility, and readable label text.

The Designer should not own data business logic or backend concerns; those remain with the Coder. The Designer's output should be reflected in `app/styles.css` and in the HTML structure in `app/index.html`.

## Coder responsibilities

The Coder owns implementation correctness and operational reliability.

Detailed responsibilities:

- Build the actual HTML shell and any minimal script logic required to render cards dynamically from `project-data.json`.
- Ensure the dashboard is runnable from the repository with working relative paths and a proper preview configuration.
- Validate JSON integrity, browser compatibility, and path correctness.
- Keep the app deterministic and easy to maintain within the repo's small static structure.
- Handle data-safe fallback behavior if the JSON file is missing or malformed.
- Confirm that the project loads successfully from `.vscode/launch.json` and opens `index.html` without exposing directory listings.

The Coder should also review the final design with an eye toward maintainability and runtime stability rather than pure aesthetic polish.

## Dependencies and sequencing

### Must be sequential

1. Project scope and dashboard requirements: The Planner and Orchestrator define the desired outcome and the file ownership before work begins.
2. Data contract definition: `app/project-data.json` should be defined before the rendering logic is finalized, otherwise the HTML/script may assume the wrong structure.
3. HTML structure approval: The page should be shaped before final CSS polish, otherwise the Designer would be styling unstable markup.
4. CSS finalization: `app/styles.css` should come after the HTML structure is in place and after the major content grouping is agreed.
5. Launch configuration: `.vscode/launch.json` should be created after the app structure and file paths are stable so it points to the correct app directory and preview target.

### Can run in parallel

- Designer can draft visual conventions and card layout in parallel with the Coder drafting the initial `project-data.json` schema once the shared data shape is loosely agreed.
- Coder can begin a skeleton `app/index.html` while the Designer prepares CSS classes and style tokens, as long as the markup remains flexible and not final to the point of blocking style implementation.
- The Orchestrator may make independent check-ins while the Designer and Coder complete their initial implementation passes, as long as they do not edit the same file without coordination.

### Forced coordination points

- The same files should not be edited simultaneously without a clear handoff: `app/index.html` and `app/styles.css` must be treated as a coordinated pair.
- The content structure in `app/index.html` and the data contract in `app/project-data.json` must agree to avoid mismatched field names or missing values.

## Edge cases and risks

1. Missing or malformed `project-data.json`
   - Risk: page shows blank or broken cards.
   - Mitigation: use graceful fallback content or a safe empty-state message when data cannot be loaded.

2. Browser loading restrictions for local files
   - Risk: if `index.html` is opened using a plain `file://` URL, fetch-based JSON loading may fail in some browsers.
   - Mitigation: preview using a local HTTP server from the `app/` directory via `.vscode/launch.json`.

3. Inconsistent data schema
   - Risk: fields like `status`, `priority`, and `progress` may be missing or mismatched.
   - Mitigation: define a clear schema early and treat unknown values as neutral defaults.

4. Overly long project names or long status strings
   - Risk: cards become visually broken or hard to scan.
   - Mitigation: use wrap behavior, truncation where appropriate, and explicit sizing rules in CSS.

5. Accessibility issues during visual polish
   - Risk: status colors can be low-contrast or inaccessible.
   - Mitigation: include text labels, strong contrast, and not rely on color alone for meaning.

6. Responsive layout breakage
   - Risk: cards collapse or become unreadable on smaller screens.
   - Mitigation: establish breakpoints early and test at typical viewport widths.

7. Empty project list
   - Risk: dashboard looks broken if there are no projects.
   - Mitigation: include a clear empty-state card with a helpful message.

8. Cross-file drift
   - Risk: HTML and CSS reflect different naming patterns.
   - Mitigation: keep class names deterministic and documented before implementation.

## Validation expectations

Validation should be split into static, runtime, and integration checks.

### Static validation

- `app/project-data.json` must parse as valid JSON.
- `.vscode/launch.json` must be strict JSON and valid for VS Code.
- `app/index.html` must reference the stylesheet with a correct relative path.
- The HTML should include clear semantic sections and deterministic class names used by CSS.
- CSS selectors should be specific enough to avoid accidental collisions with unrelated page elements.
- All referenced files should exist in the `app/` directory and be valid relative paths.

Concrete commands or checks:

- `python -m json.tool app/project-data.json`
- Validate `.vscode/launch.json` by confirming it parses as JSON in a local editor or via a JSON-aware tool.
- Open the HTML file in a browser or use a static preview tool to catch path issues and markup errors.

### Runtime validation

- Launch the app from the `app/` directory via the configured preview setup.
- Confirm the dashboard loads from `http://127.0.0.1:<port>/index.html` or equivalent local preview URL.
- Verify there are no console errors when the page loads.
- Ensure that `project-data.json` is fetched successfully and cards render without blank containers.
- Check that the page works when the JSON file contains typical values and at least one edge-case item.

Concrete expectations:

- No 404s for CSS or JSON files.
- No uncaught JavaScript errors.
- Browser console is free of missing resource or parsing errors.
- Local preview opens the dashboard, not a directory listing.

### Integration validation

- The page clearly presents a dashboard experience on first view, with stable spacing, strong hierarchy, and recognizable product patterns.
- Each project card includes the key information in a readable layout: name, status, priority, progress, or due date.
- Status badges are visually consistent and color-coded without relying on color alone.
- The layout remains usable across desktop, tablet, and phone widths.
- The app remains coherent when the data set grows from a few projects to a larger set.

Acceptance criteria:

- The dashboard appears polished and product-ready.
- The `app/` folder contains the structural HTML, CSS, and data contract needed to render the dashboard.
- The local preview works from VS Code using `.vscode/launch.json`.
- The repository remains clean and no unrelated files are modified.

## Recommended execution sequence

1. Planner confirms scope and constraints using the repository's custom agents and project task context.
2. Designer drafts the visual system and the overall dashboard information hierarchy.
3. Coder creates the data contract in `app/project-data.json` and starts the skeleton HTML in `app/index.html`.
4. Designer finalizes `app/styles.css` to match the agreed structure and the product vision.
5. Coder verifies the HTML, JSON, and paths all work together.
6. Coder adds the final `.vscode/launch.json` preview configuration.
7. Orchestrator performs final integration review against the repository's agent rules and verifies the application is ready to open.

## Final risk statement

This is a deliberately low-complexity dashboard implementation, so the main risks are not technical complexity but file coordination and preview reliability. The repository's custom agent model reduces that risk by assigning explicit ownership, defining a clear sequence, and treating integration validation as a required final step. If any file path, JSON schema, or naming mismatch emerges during implementation, the plan intentionally prioritizes a stable, deterministic correction path over a fast but brittle implementation.
