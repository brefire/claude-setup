Document changes to the .claude/ configuration directory.
This command is for tracking modifications to principles,
commands, agents, and skills — not project code.

1. CHECK the current state of the .claude/ directory against
   the most recent git diff or working tree changes.

2. PRODUCE a dated changelog entry using the format defined in
   CHANGELOG_FORMAT.md (same directory as this command). Use
   only the sections and structure specified there.

3. If CHANGELOG.md does not exist at ~/.claude/CHANGELOG.md
   (or .claude/CHANGELOG.md in the workspace):
   - Create CHANGELOG.md with the new entry as its only content.
   - Confirm the file was created and show the entry.

4. If CHANGELOG.md already exists:
   - Prepend the new entry above the existing content (newest first).
   - Write the updated content to CHANGELOG.md.
   - Show me the addition that was prepended.

5. Do not modify any other files. This command only creates or
   updates CHANGELOG.md with the new entry.

Target: $ARGUMENTS
