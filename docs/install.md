Install opencode-commander via git clone and symlink.

## STEPS

1. Ask the user: "Install globally (all projects) or per-project?" Default to global if no answer.
   - Global: clone to `~/.opencode-commander`, symlink to `~/.config/opencode/commands/`
   - Per-project: clone to `.opencode/vendor/opencode-commander`, symlink to `.opencode/commands/`

2. Clone the repository:
   ```bash
   git clone https://github.com/Dawnfz-Lenfeng/opencode-commander.git <clone-dir>
   ```

3. Create the commands directory if it doesn't exist, then symlink all command files:
   ```bash
   mkdir -p <commands-dir>
   ln -sf <clone-dir>/commands/*.md <commands-dir>/
   ```

4. Verify each symlink resolves to a non-empty file.

5. Report what was installed:
   - Read the `description` field from each file's frontmatter
   - List each command name + its description
   - Tell the user to restart opencode for commands to take effect

## UPDATE

```bash
cd <clone-dir> && git pull
```

Symlinks point to the same files, so they update automatically.
