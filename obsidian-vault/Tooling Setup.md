# Tooling Setup

The durable record of the tools and integrations configured for this repository.
Prepared 2026-09-28.

---

## 1. Obsidian

Obsidian is a desktop GUI application. The cloud container that Claude Code runs in is
headless and ephemeral, so Obsidian is not installed there. Instead, this repository
carries a ready configured vault at `obsidian-vault/`, and you install the application on
your own machine.

### Install the latest stable Obsidian
- **macOS**: `brew install --cask obsidian` or download the `.dmg` from https://obsidian.md/download
- **Windows**: `winget install Obsidian.Obsidian` or download the installer from https://obsidian.md/download
- **Linux**: download the `.AppImage`, `.deb`, or `.snap` from https://obsidian.md/download
  - Snap: `sudo snap install obsidian --classic`
  - Flatpak: `flatpak install flathub md.obsidian.Obsidian`

### Open the vault
1. Launch Obsidian.
2. Choose **Open folder as vault**.
3. Select the `obsidian-vault` directory in this repository.

### Vault configuration committed here
- `.obsidian/app.json`: live preview, attachment folder, preview default view.
- `.obsidian/appearance.json`: default Obsidian theme.
- `.obsidian/core-plugins.json`: file explorer, search, graph, backlinks, tags, daily
  notes, templates, command palette, outline, bookmarks, and more enabled.
- `.obsidian/community-plugins.json`: none, kept empty by design.

---

## 2. Claude Code

Already installed and operational in the cloud environment. No installation was required.

| Item | Value |
|---|---|
| Binary | `/opt/claude-code/bin/claude` |
| Version | 2.1.283 |
| Node runtimes | `/opt/node20`, `/opt/node21`, `/opt/node22` (node v22.22.2, npm 10.9.7) |
| User config | `/root/.claude.json`, `/root/.claude/` |

### Skills available to Claude Code
Synced under `/root/.claude/skills/`: docx, pdf, pptx, xlsx, skill-creator, deep-research,
import-memory, computer-use, chrome-browser, built-in-browser, docs, session-start-hook,
plus the repository skill added below. Verify at any time with `/skills` or by listing
`.claude/skills/` in the repository.

---

## 3. 21st.dev Magic MCP server

Configured at the repository root in `.mcp.json` so it travels with the project and loads
in every session.

```json
{
  "mcpServers": {
    "magic": {
      "command": "npx",
      "args": ["-y", "@21st-dev/magic@latest"],
      "env": { "API_KEY": "${API_KEY:-}" }
    }
  }
}
```

- Package: `@21st-dev/magic`, latest stable **0.2.3** (confirmed on the npm registry). The
  package is now a shim that proxies to `https://21st.dev/api/mcp`.
- Runtime: launched on demand via `npx`; the Node toolchain is present.
- Server key: `magic`. Its tools appear under the `mcp__magic__*` namespace once connected.

### Verified operational status
A stdio smoke test confirmed the server downloads, boots (shim v0.2.3), speaks MCP
JSON-RPC, and reaches the 21st.dev API through the environment proxy. The `initialize`
handshake returned a well-formed response. Unauthenticated, it returns
`-32001 Not authenticated`, which is the expected state until a key is supplied.

### Remaining step: supply the API key
The server is configured **unauthenticated** by decision. It registers but every tool call
returns `-32001 Not authenticated` until a key is present.

1. Obtain a fresh key at https://21st.dev/mcp (keys from the old "Magic" service were
   reset and must be reissued).
2. Add it as a durable environment secret named `API_KEY` in the Claude Code environment
   settings (the container is ephemeral, so a secret is the only place a key survives). The
   server also accepts `TWENTY_FIRST_API_KEY` or `API_KEY_21ST`.
3. Reload the session. The `magic` server then authenticates automatically.

Verify connection with `claude mcp list` (expects `magic` present) and, once the key is
set, by calling a `mcp__magic__*` tool.

---

## 4. 21st.dev skill

Authored at `.claude/skills/21st-dev-magic/SKILL.md`, project scoped so it commits with the
repository and is callable from Claude Code. It drives the Magic MCP tools to generate and
refine UI components and documents the auth prerequisite and known gotchas.

Verify with `/skills` or by listing `.claude/skills/`.

---

## Verification checklist
- [x] Claude Code present and versioned (2.1.283).
- [x] Node toolchain present (v22.22.2 / npm 10.9.7).
- [x] `@21st-dev/magic` reachable on npm (0.2.3).
- [x] `.mcp.json` written with the `magic` server.
- [x] `.claude/skills/21st-dev-magic/SKILL.md` written.
- [x] Obsidian vault committed at `obsidian-vault/`.
- [ ] `API_KEY` secret supplied (your action).
- [ ] Obsidian installed on your machine and vault opened (your action).
