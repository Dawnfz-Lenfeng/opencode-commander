# Install

Install opencode-commander by downloading the command files from GitHub.

## STEPS

1. Create the commands directory if it doesn't exist:
   - Global: `~/.config/opencode/commands/`
   - Ask the user: "Install globally (all projects) or per-project?" Default to global if no answer.

2. Download each command file using curl:
   ```bash
   curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/master/commands/debate.md -o <commands-dir>/debate.md
   curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/master/commands/audit.md -o <commands-dir>/audit.md
   curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/master/commands/simplify.md -o <commands-dir>/simplify.md
   ```

3. Verify each file was downloaded successfully (check file exists and is non-empty).

4. Report what was installed:
   - List the installed commands with their descriptions
   - Tell the user to restart opencode for commands to take effect
   - Show usage examples for each command
