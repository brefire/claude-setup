Analyze this project and help me create a project-level CLAUDE.md.
Do not create the file — produce the content for my review.

1. SCAN the codebase and identify:
   - Primary language(s) and framework(s).
   - Package manager and key dependencies.
   - Directory structure and organization pattern.
   - Test framework and test file conventions.
   - Linting and formatting configuration.
   - Build and run commands.
   - Existing code patterns (component structure, naming conventions,
     state management, API patterns, error handling patterns).

2. PRODUCE a project CLAUDE.md with the following sections:

   ## Project Overview
   One-line description of what this project is.

   ## Stack
   Language, framework, key libraries.

   ## Directory Structure
   Brief map of where things live.

   ## Commands
   Build, dev, test, lint — whatever is configured.

   ## Conventions
   Naming patterns, file organization, component patterns,
   typing conventions observed in the existing code.

   ## Patterns to Follow
   Specific patterns found in the codebase that new code
   should be consistent with. Include file paths as examples.

   ## Do Not
   Project-specific constraints based on what you observe
   (e.g., "Do not use CSS modules — this project uses Tailwind"
   or "Do not create barrel exports — this project imports directly").

3. FLAG anything you could not determine and state what
   information I need to provide.

Present the content for my review. I will tell you when to
create the file.