---
name: install-terminal-statusline
description: Install the Claude Code status line that shows project, model, a context bar with a red marker at 100K tokens, and tokens used in the terminal. Writes ~/.claude/statusline.sh and registers it in ~/.claude/settings.json. Use when the user asks to install, restore or fix the terminal status line, or sees no tokens in the terminal.
---

# Install terminal status line

Writes the status line script and registers it in the user settings. The whole
script is in this file: nothing is copied from anywhere else.

1. **Write the script.** Create `~/.claude/statusline.sh` with exactly the
   content below and make it executable (`chmod +x`). If the file already
   exists and differs, show a short diff and ask once before overwriting. If it
   is a symlink, remove the link first and write a regular file.
2. **Register it.** In `~/.claude/settings.json` set `statusLine` to
   `{"type": "command", "command": "~/.claude/statusline.sh"}`. Merge with
   `jq` and keep every other key. If the file doesn't exist, create it with
   only that key. If `statusLine` already has that value, change nothing.
3. **Check it.** Run the script with this sample input and show the output:
   `{"workspace":{"project_dir":"/a/b/demo"},"model":{"display_name":"M"},`
   `"context_window":{"total_input_tokens":1500,"total_output_tokens":20,`
   `"context_window_size":200000}}`
   It must print
   `demo · M · ░░░░░░░░░░▼░░░░░░░░░░ ↑1.5k ↓20`, with the ▼ in red. If `jq` is
   missing, say so and stop.
4. **Report** in one line whether the script and the setting were created,
   updated or already in place. The status line shows up in the next session.

The bar spans the whole context window. The red ▼ marks 100K tokens, where
the quality of the answers starts to drop, and the part of the bar past it
turns red.

```bash
#!/usr/bin/env bash
# Claude Code status line: project, model, context bar and tokens used.
# Claude Code sends the session data as JSON on stdin.
input=$(cat)

project=$(echo "$input" |
  jq -r '(.workspace.project_dir // .cwd // "") | split("/") | (last // "")')
model=$(echo "$input" | jq -r '.model.display_name // "?"')
tokens_in=$(echo "$input" | jq -r '.context_window.total_input_tokens // 0')
tokens_out=$(echo "$input" | jq -r '.context_window.total_output_tokens // 0')
size=$(echo "$input" | jq -r '.context_window.context_window_size // 200000')
[ "$size" -gt 0 ] 2>/dev/null || size=200000

width=20          # cells in the bar
dumb_zone=100000  # tokens where the red arrow sits
red=$'\033[31m'
reset=$'\033[0m'

# 1234 -> 1.2k, 1234567 -> 1.2M
fmt() {
  awk -v n="$1" 'BEGIN {
    if (n >= 1000000) printf "%.1fM", n / 1000000
    else if (n >= 1000) printf "%.1fk", n / 1000
    else printf "%d", n
  }'
}

# Filled cells up to the used tokens, red past the arrow, arrow at 100K.
marker=$(( dumb_zone * width / size ))
filled=$(( tokens_in * width / size ))
bar=""
for (( i = 0; i < width; i++ )); do
  [ "$i" -eq "$marker" ] && bar+="${red}▼${reset}"
  if [ "$i" -ge "$filled" ]; then
    bar+="░"
  elif [ "$i" -ge "$marker" ]; then
    bar+="${red}█${reset}"
  else
    bar+="█"
  fi
done

echo "$project · $model · $bar ↑$(fmt "$tokens_in") ↓$(fmt "$tokens_out")"
```
