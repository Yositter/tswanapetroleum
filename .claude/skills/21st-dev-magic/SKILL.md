---
name: 21st-dev-magic
description: Generate, refine, and source production-ready UI components via the 21st.dev Magic MCP server. Use whenever the task involves creating a new UI component, a React/Vue/HTML interface element, a landing-page section, a form, a navbar, a hero, a pricing table, or any front-end visual building block, or when the user references "21st.dev", "Magic", "/ui", or "/21", or asks to browse or fetch a component from the 21st.dev registry. Produces framework-appropriate component code and can pull logos and inspiration from the 21st.dev catalog.
---

# 21st.dev Magic

## Purpose
The Magic MCP server (`@21st-dev/magic`) turns a natural-language description into a
production-ready UI component drawn from the 21st.dev registry. It also fetches brand
logos and lets you inspect existing catalog components before adopting them.

## Prerequisites
- The `magic` MCP server must be running and connected. Its definition lives in the
  repository at `.mcp.json`.
- A 21st.dev API key must be present in the `API_KEY` environment variable (the server
  also accepts `TWENTY_FIRST_API_KEY` or `API_KEY_21ST`). Without it the server registers
  but every tool call returns a `-32001 Not authenticated` error. Obtain a fresh key at
  https://21st.dev/mcp and supply it as a durable environment secret. Keys issued for the
  older "Magic" service were reset, so a legacy key must be reissued.

## When to invoke
Invoke this skill when the request is any of:
- "Build me a <component>" for a web interface (hero, navbar, pricing, footer, form, card, modal, dashboard tile).
- A message beginning with `/ui`, `/21`, or naming "21st.dev" or "Magic".
- A request to fetch a company logo in JSX/SVG for the UI.
- A request to browse, search, or reuse a component from the 21st.dev catalog.

## Available MCP tools
Once connected, the server exposes tools under the `mcp__magic__*` namespace. Names may
evolve with the package; list the live tool set before relying on a specific name. Typical
capabilities:
- **Component builder**: accepts a description and target framework, returns component code.
- **Component refiner / inspiration**: retrieves matching catalog entries to adapt.
- **Logo search**: returns a brand logo as an inline SVG or component.

## Working method
1. Confirm the `magic` server is connected. If tool calls fail with an auth error, stop
   and tell the user the `API_KEY` secret is missing, with the console link above. Do not
   fabricate component code as a silent substitute.
2. State the target framework and styling system before generating (React + Tailwind is
   the Magic default; confirm if the repository uses something else).
3. Generate the component, then integrate it into the repository's existing structure and
   conventions rather than dropping an isolated file.
4. Verify the result renders: run the project's build or dev server where one exists, or
   at minimum type-check the generated code.

## Gotchas
- No `API_KEY` means every call fails. Check this first when a tool errors.
- The server runs via `npx -y @21st-dev/magic@latest`, so the first call in a fresh
  container downloads the package. Allow for that latency and ensure the npm registry is
  reachable through the environment's network policy.
- Generated components assume a modern bundler and may pull in dependencies (e.g.
  `framer-motion`, `lucide-react`). Install them before declaring the component working.
- The catalog is remote. If the environment's network policy blocks 21st.dev, catalog and
  logo tools fail while purely generative calls may still succeed.
