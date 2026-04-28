Install opencode-commander by downloading the command files from GitHub.

## STEPS

1. Fetch the command index from GitHub:
   ```bash
   curl -fsSL https://api.github.com/repos/Dawnfz-Lenfeng/opencode-commander/contents/commands | python3 -c "import sys,json; [print(f['name']) for f in json.load(sys.stdin) if f['name'].endswith('.md')]"
   ```
   This lists all available command files dynamically.

2. Ask the user: "Install globally (all projects) or per-project?" Default to global if no answer.
   - Global: `~/.config/opencode/commands/`
   - Per-project: `.opencode/commands/`
   Create the directory if it doesn't exist.

3. Download each command file:
   ```bash
   curl -fsSL https://raw.githubusercontent.com/Dawnfz-Lenfeng/opencode-commander/master/commands/<filename> -o <commands-dir>/<filename>
   ```
   Run this for every `.md` file found in step 1.

4. Verify each file was downloaded successfully (check file exists and is non-empty).

5. Report what was installed:
   - Read the `description` field from each downloaded file's frontmatter
   - List each command name + its description
   - Tell the user to restart opencode for commands to take effect
