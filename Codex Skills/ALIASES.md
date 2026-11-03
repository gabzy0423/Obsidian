# Personal skill aliases

Use these short skill names with Codex's `$` skill picker. They are discoverable skills, not native `/` commands. Quoted examples, code blocks, file paths, and discussion of aliases do not invoke them.

| Shortcut | Skill folder | Workflow |
|---|---|---|
| `$ui` | `ui` | `ui-ux-pro-max` |
| `$qa` | `qa` | `qa-testing` |
| `$code` | `code` | `agentic-coding` |
| `$grill` | `grill` | `grill-with-docs` |
| `$handoff` | `handoff` | Existing handoff workflow |
| `$pickup` | `pickup` | Existing pickup workflow |

Before acting, read the mapped folder's `SKILL.md` relative to this file, then follow that skill and its required resources. Pass the rest of the user's request as the task; use conversation context for an omitted task when clear. If multiple aliases are explicitly requested, follow the requested order. If a skill cannot be read, report the missing path instead of inventing its workflow.

An alias selects a workflow; it does not bypass project instructions, the skill's scope, permissions, or implementation gates. `$qa` retains its GoPasig-specific scope. `$grill` performs requirements questioning and project documentation, without application implementation. `$ui` selects UI/UX guidance; use the user's wording to determine whether they want advice, design, or UI changes. `$handoff` and `$pickup` retain their configured continuity-note location.
