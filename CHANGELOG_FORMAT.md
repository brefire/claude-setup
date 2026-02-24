# CHANGELOG format

Use this structure for every entry in CHANGELOG.md. Entries are prepended (newest first).

## Structure

```markdown
## YYYY-MM-DD

### Added
- `path/to/file`: one-line description of what was added.

### Modified
- `path/to/file`: one-line description of what changed and why.

### Deleted
- `path/to/file`: one-line description of what was removed and why.

### Notes
Optional paragraph(s) on why these configuration changes were made.
```

## Rules

- **Date**: `## YYYY-MM-DD` (ISO date, one heading per entry).
- **Sections**: Use only `### Added`, `### Modified`, `### Deleted`, `### Notes`. Omit a section if it has no items.
- **Bullets**: One line per change; format `- \`path/to/file\`: description.`
- **Notes**: Free-form; can be multiple lines. No bullet list required.
- **Scope**: Only changes under the .claude/ configuration directory (principles, commands, agents, skills). Not project code.
