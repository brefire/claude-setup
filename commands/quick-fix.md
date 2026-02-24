Apply a small, isolated correction. This command is for trivial
changes where the full verification loop is unnecessary.

A quick-fix is NOT a task. It is a correction. Valid quick-fixes:
- Renaming a variable, function, or file.
- Fixing a typo in code or copy.
- Correcting an import path.
- Removing dead code that is clearly unused.
- Fixing a type error or lint violation.

A quick-fix is NOT valid for:
- Adding new functionality or behavior.
- Modifying logic or control flow.
- Changes that affect more than 3 files.
- Changes that require installing or modifying dependencies.
- Anything that requires a decision about how to implement it.

If this does not qualify as a quick-fix, stop and say:
"This exceeds quick-fix scope. Use /implement or /breakdown instead."
State why it does not qualify.

When applying a quick-fix:
1. State what you will change and where (one line).
2. Make the change.
3. Confirm what was changed (one line).

Fix: $ARGUMENTS