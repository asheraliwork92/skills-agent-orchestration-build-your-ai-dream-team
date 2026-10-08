# Project Pulse Dashboard Implementation Plan

## Goal and scope

Build Mona's lightweight, contributor-friendly Project Pulse dashboard as a static HTML/CSS/JSON app. Contributors should be able to scan multiple projects and understand each project's name, owner, status, recent activity, priority or risk, and a short summary. The first view should feel like a polished dashboard rather than a plain page or server directory listing.

The app is currently empty. Keep the implementation within the requested deliverables:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`

Do not add a framework, build system, package dependencies, or backend. `.vscode/launch.json` is support configuration for the runnable static app.

## Team responsibilities

- **Orchestrator:** Use this plan to delegate work in phases, keep file ownership explicit, manage dependencies and any overlap, then review the integrated app and runtime preview.
- **Planner:** Research repository requirements and edge cases, record this plan, and identify validation criteria before implementation.
- **Designer:** Define the information hierarchy, visual direction, responsive behavior, and accessibility requirements. Advise on visible cards, status badges, priority treatment, typography, spacing, and loading/error/empty states. Keep design recommendations separate from implementation files to avoid overlapping edits.
- **Coder:** Implement only the files explicitly assigned in the build phase. Connect the HTML to the stylesheet and JSON data, render the project cards, create the JSON dataset, configure the preview launch, and report validation and remaining risks.

## Ordered implementation steps

### 1. Confirm requirements and assign ownership

The Orchestrator reads this plan and `.github/project-pulse-brief.md`, confirms the four deliverable paths, and assigns the Designer and Coder their scopes below. The Coder owns the implementation files; the Designer provides design guidance without editing those files.

**Files:** Read-only review of `.github/project-pulse-brief.md`, `.github/agents/`, and this plan. No implementation edits.

### 2. Produce design guidance

Designer recommends a responsive card-based layout with a clear page heading, visible project names, owner and summary, status badges, recent activity, and visually distinct priority/risk indicators. Specify accessible contrast, meaningful heading order, readable type and spacing, and responsive behavior at narrow widths. Also recommend how loading, malformed/unavailable data, and an empty project list should be communicated.

**Files:** No implementation files. Deliver recommendations to the Orchestrator/Coder.

### 3. Implement and connect the static dashboard

Coder implements the following assigned files:

| File | Assignment and acceptance details |
| --- | --- |
| `app/project-data.json` | Create valid JSON with a top-level `projects` array and multiple realistic sample projects. Every project object must include `name`, `owner`, `status`, `recentActivity`, and `priority`; include a concise contributor-friendly `summary` as well. Use clear, consistent string values and avoid relying on a particular project count in the rendering logic. |
| `app/index.html` | Create the semantic page and use the exact document title **Project Pulse**. Link `styles.css` and load `project-data.json` as the source of project data. Render one visible element with class `project-card` per project, including name, owner, status, recent activity, priority, and summary. Include a clear loading state and helpful error/empty states. Use accessible headings, labels, and status/priority text that does not depend on color alone. Keep data rendering simple and dependency-free; no extra application files are in scope. |
| `app/styles.css` | Create the polished responsive dashboard styling based on Designer guidance. Include the required `.dashboard` and `.project-card` selectors, plus visible status/priority treatments, readable spacing, and responsive layout. Use `border-radius` and `box-shadow` for card polish, while maintaining adequate contrast and visible focus behavior if interactive elements are present. |
| `.vscode/launch.json` | Create strict JSON with no comments. Add the launch configuration named **Run Project Pulse Dashboard**. Serve the app from `${workspaceFolder}/app` with `python3 -m http.server 5500`, and configure `serverReadyAction` to open `http://localhost:%s/index.html` so preview opens the page rather than the directory root. Use a launch type that supports the shell command and automatic browser opening in the Codespace. |

The HTML must fetch the JSON over HTTP; opening `index.html` directly as a `file://` URL can block data loading in browsers. The launch configuration is therefore part of the working app, not optional polish.

### 4. Integrate and review

Orchestrator checks that the HTML paths match the actual stylesheet/data files, that data fields are rendered (not merely present in source), and that the launch configuration serves from the app directory and opens the page. Resolve any mismatch with the file owner before previewing.

**Files:** Review the four implementation files. Any fixes remain with the Coder and within the assigned paths.

### 5. Validate and hand off

Run the checks below, launch the dashboard in VS Code using **Run Project Pulse Dashboard**, inspect the browser preview at the opened `/index.html` URL, and stop the server after review. Report what was checked and any remaining limitation.

## Dependencies and parallel-work decisions

- Requirement review precedes implementation so file ownership and acceptance criteria are clear.
- Designer's recommendations should arrive before finalizing visual details in `app/styles.css` and the presentation structure in `app/index.html`. Designer advice can be developed while the Coder prepares the data schema and launch configuration.
- The Coder may create `app/project-data.json` and `.vscode/launch.json` in parallel: neither depends on the other's content. The JSON schema and preview command are already specified here.
- HTML rendering depends on the agreed JSON field names and loading behavior. It can begin against the specified schema, but integration validation must wait until `project-data.json` exists.
- Final CSS details depend on the Designer's direction and the HTML's chosen structure/classes. Coder owns both implementation files to avoid concurrent edits; do not run separate Designer and Coder edits against `app/styles.css` or `app/index.html`.
- Integration checks and runtime preview must happen only after all four assigned files are present. The preview depends on the launch configuration, the server being able to serve the app directory, and the page successfully fetching the JSON.

## Edge cases and risks

- **Missing, invalid, or unavailable JSON:** Show a readable error instead of leaving a permanent loading message or silently rendering nothing.
- **Empty `projects` array:** Show a useful empty-state message; do not display a broken or blank card grid.
- **Incomplete project object:** Avoid broken markup for a missing optional `summary`; handle required fields defensively and report/fallback clearly rather than showing `undefined`.
- **Unexpected status or priority value:** Keep the value readable and avoid styling that assumes only one fixed set of statuses or priorities. Convey meaning with text as well as color.
- **Long names or activity descriptions:** Allow text wrapping without horizontal overflow or clipped card content, including at narrow viewport sizes.
- **Browser access to JSON:** A `file://` preview may block `fetch`; use the configured local HTTP server and verify the launched page's request succeeds.
- **Port 5500 already in use:** The server may fail to start. Stop the conflicting process or choose and consistently update the port, URL pattern, and URL expectation before validation.
- **Launch configuration compatibility:** Confirm the selected VS Code launch type supports `command`, `cwd`, and `serverReadyAction` in the Codespace. Keep launch JSON strictly valid, with no comments or trailing commas.
- **Scope creep:** Keep the app static and dependency-free; do not add backend services, unnecessary tooling, or files outside the assigned scope.

## Validation expectations

Validate implementation and runtime behavior, not just the existence of files:

1. **JSON syntax and schema**
   - Run `python3 -m json.tool app/project-data.json` and `python3 -m json.tool .vscode/launch.json`; both must parse successfully.
   - Inspect `project-data.json` to confirm the top-level `projects` value is an array, includes multiple sample entries, and each entry contains `name`, `owner`, `status`, `recentActivity`, and `priority` (plus the planned short `summary`).
   - Confirm `launch.json` is strict JSON, contains **Run Project Pulse Dashboard**, uses `${workspaceFolder}/app`, runs `python3 -m http.server 5500`, and opens `http://localhost:%s/index.html`.
2. **Static integration**
   - Confirm `app/index.html` has the exact title **Project Pulse**, references `styles.css` and `project-data.json`, and displays project cards with the `project-card` class.
   - Confirm the page renders each project's required fields and summary from loaded JSON, and provides understandable loading, failure, and empty states.
   - Confirm `app/styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`; review responsive behavior, readable contrast, and wrapping of long content.
3. **Runtime preview**
   - In VS Code Run and Debug, start **Run Project Pulse Dashboard**. Confirm the server starts from `app/`, the browser opens the `/index.html` URL, and the dashboard—not a directory listing—is visible.
   - Confirm multiple cards show their data from JSON, the browser console/network view has no JSON-fetch or rendering errors, and the layout remains usable at a narrow viewport.
   - Verify the empty/error presentation when practical (for example, temporarily test a copied/modified local state without retaining unintended changes), then restore the valid sample data.
   - Stop the preview server and report the result, including any port or environment-specific limitation.

## Open questions and assumptions

- The brief does not prescribe exact sample project names, colors, or status/priority vocabularies. Use representative, non-sensitive sample data and consistent labels.
- The brief requests a contributor-friendly summary but does not list its exact property name; this plan uses `summary` as an additional project field.
- No additional data source, editing interaction, persistence, or framework is required; assume the dashboard is a read-only static preview.
